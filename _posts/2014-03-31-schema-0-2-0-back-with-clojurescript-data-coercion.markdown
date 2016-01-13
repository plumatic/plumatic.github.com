---
layout: post
title: 'Schema 0.2.0: back with Clojure(Script) data coercion'
date: 2014-03-31 14:56:52.000000000 -07:00
---
*tl;dr: [Schema](https://github.com/plumatic/schema) 0.2.0 is here, adding support for data coercion and 5x faster validation.  We've also added Schema support in [Plumbing and Graph 0.2.0](https://github.com/plumatic/plumbing). Join the discussion on Hacker News and let us know what you think.*

Back in September, we [released](http://plumatic.github.io/schema-for-clojurescript-data-shape-declaration-and-validation/) the first version of [Schema](https://github.com/plumatic/schema), and we've been blown away by the interest and contributions from the community since.  Schemas are declarative descriptions of data shapes that make it easy to document and validate Clojure(Script) data. We use Schemas all over our codebase, and others seem to share our finding that Schemas can make Clojure development clearer, faster, and more fun.

In addition to a variety of small bugfixes, improvements, and a huge performance boost (see the [Changelog](https://github.com/plumatic/schema/blob/master/CHANGELOG.md)), version 0.2.0 brings something new to the party: **transformations**, which provide a way to perform structured manipulation of data using rules cued by Schemas.

## Why Transformations?

One reason we built Schema was to make sure our backend API servers send and receive properly formed data when communicating with our iOS and web clients.  But when we turned on validation, we were in for some nasty surprises.

<center><img width="300px" src="http://www.roflcat.com/images/cats/That_s_Not_What_I_Ordered.jpg"></center>

Clojure has a much more nuanced system of data types than JSON, and so simple JSON parsing of inputs did not always produce the data we ordered.  Some of these issues were expected; JSON doesn't have keywords, so we've become accustomed to writing lots of fiddly boilerplate code for updating nested data structures to convert particular Strings to Keywords.  Others were a bit more of a surprise, such as when Schema validation on a `Double` member failed because the value sent by the client happened to be exactly 173.0, and was parsed as an `Integer` (JSON doesn't distinguish between integers and floating point values).

This state of affairs was very frustrating: we know exactly the format in which we wanted our input data -- and we had already written it down precisely using Schemas -- but were still stuck writing lots of boilerplate code to get the data into the right shape.  

<a href="http://xkcd.com/1205/"><center><img width="400px" src="http://imgs.xkcd.com/comics/is_it_worth_the_time.png"></center></a>

Spending time writing the boilerplate conversions became exceedingly inefficient, so we decided to do something about it.  If schemas have all the information needed to do what we want, then we should be able to solve this problem once and for all and do away with the boilerplate.  And that's what we're delivering out of the box in Schema 0.2.0:  a completely automated, safe way to coerce JSON and query params using just your existing Schemas, with no additional code required.  No matter how deeply nested those `s/Keyword` Schemas are, the corresponding input `String`s will be automatically converted during validation.

And the fun doesn't stop at coercion.  Schema is now based on a general `walker` protocol that allows for structured Schema-driven data transformations, of which validation and input coercion are just two simple examples.  This abstraction is inspired by the excellent data transformation facilities of `clojure.walk`; the difference is that whereas `clojure.walk` operates on free-form data, `schema/walker` allows you to define transformations that depend on *both* the Schema and data at a particular place in a data structure via a parallel walk.  To `clojure.walk` a String is just a String; but `schema/walker` knows whether this string is *supposed to be* a String, Keyword, Number, or FooBar, and can act accordingly.

It's easy to write your own [custom transformations](https://github.com/plumatic/schema/wiki/Writing-Custom-Transformations). We're excited to see what other applications for transformation the community dreams up!

## Show Me The Code

The rest of this post describes this new functionality in more detail, with examples.  We start with a brief recap of Schema definition and validation, using an example that we'll build on throughout the post:

<script src="https://gist.github.com/w01fe/8246933.js"></script>

`CommentRequest` is a schema for data a client might send to the API to post a comment and share it to the provided external networks.  The `parent-comment-id` field is optional, and is only present if the comment is a reply.

`+good-request+` matches the schema and passes validation, but `+bad-request+` has several issues that are clearly explained in the validation exception.  (For more Schema examples, check out the [readme](https://github.com/plumatic/schema).)  

### Coercion

Runtime schema validation is a valuable tool for pinpointing mismatches between your expectations and your real data.  Sometimes, this assurance that your data is correct is all that's needed.  But in other cases, mismatches are actually *anticipated*, and rather than throw up your hands, you'd like to actually *fix* the data and get on with the task at hand.

For example, our backend provides a JSON API for use by iOS and web clients.  One of the methods allow a user to post a comment on a story. The request body might look something like this:

    {"parent-comment-id": 2128123123, "text": "This is awesome!", "share-services": ["twitter" "facebook"]}

On the backend (with the appropriate [Ring](https://github.com/ring-clojure/ring) middleware) this will show up as the Clojure data structure `+bad-request+` above.  This is almost, but not quite, what we want: an instance of the `CommentRequest` schema.  To resolve the inconsistencies, we can write some fiddly code for traversing and updating the request:

<script src="https://gist.github.com/w01fe/8248664.js"></script>

This works but writing such code gets old fast, especially when the same data types show up (possibly deeply nested) across many request types.  It is especially frustrating since this seems to be just restating the `CommentRequest` schema in code: if `parent-comment-id` is present, it must be a long; and `share-services` must be a list of service keywords.  

In fact, this is the key idea motivating schema transformations. In cases like these, the schema already contains the information needed to coerce the data into a format that validates:

<script src="https://gist.github.com/w01fe/8248688.js"></script>

Here, the `coercer` makes a single pass over the request, simultaneously coercing values and validating that the final request is a legal `CommentRequest`.  The coercions are provided by `json-coercion-matcher`, which has some useful defaults for coercing from JSON, such as:

 - Numbers should be coerced to the expected type, if this can be done without losing precision
 - When a Keyword is expected, a String can be coerced to the correct type by calling `keyword` on it
 
There's nothing special about `json-coercion-matcher` though; it's just as easy to make your own schema-specific transformations to do even more.  For example, many of our JSON API responses include `Comment` objects.  Our backend data model includes a `Comment` record with a `user-id` field, but for presentation to the client, a `Comment` must be expanded out into a more complex (potentially API-version-dependant) `ClientComment` that transforms the `user-id` into a full-fledged `ClientUser` with a username and profile image.  Accomplishing this previously required injecting resources to clientize a `Comment` (username lookup, API version, etc.) into every function that generated a response containing a `Comment`.

With schema transformations, we can just create a coercer for `ClientComment`:

<script src="https://gist.github.com/w01fe/8249101.js"></script>

and apply it when validating API responses, so that all API methods can return backend `Comment` objects (at arbitrary nesting levels), and clientization happens automatically. 

In our production API service, we annotate all of our API methods with schema metadata, provide a pluggable multimethod for defining coercions, and all of this input and output coercion and validation happens automatically with zero user-level code.  Stay tuned for an open-source release showcasing this in the near future. 

### Under The Hood

Schema is implemented using [protocols](http://clojure.org/protocols).  Previously, the workhorse of Schema was a recursive protocol method called `check`, which simultaneously traversed a schema and datum, returning `nil` for successful validation or an error description for failure.  For example, here's the old implementation of the `both` schema, which checks that a value matches multiple schemas:

<script src="https://gist.github.com/w01fe/8249853.js"></script>

This was a natural and elegant way for expressing validation logic, but that's all it could do; if you wanted to implement something more, you were stuck re-implementing all of the logic for walking a schema and data.

<center><img width="400px" src="http://static4.wikia.nocookie.net/__cb20080929232716/starwars/images/7/7f/AT-AT_egvv.jpg"></center>

In version `0.2.0`, `check` has been replaced by a new method `walker` that provides hooks to allow reuse of the traversal logic for other purposes. After switching to `walker`, here is the implementation of the `both` schema:

<script src="https://gist.github.com/w01fe/8249876.js"></script>

The first key difference is that `walker` does not take a datum, but returns a *function* that takes a datum.  This is primarily for performance: unlike in `check`, polymorphic protocol dispatch and schema parsing only happens once while walking the schema, rather than for each data element that is encountered.  In some simple tests, this yields **5x** faster validation.  

The two other, more interesting changes are:

 - The function returned by `walker` returns a walked version of `x` for success, rather than `nil`.  Validation errors are now distinguished by wrapping them in an `error` container.
 - `walker` calls a function `subschema-walker` on its subschemas, rather than recursively calling itself directly.  For the case of simple validation, `subschema-walker` is just bound to `walker`.

The first change enables applications like transformation, which require the ability to return a transformed version of the data.  The second change is what makes the walk *pluggable*: for example, `coercer` simply rebinds `subschema-walker` to a function that first applies any applicable coercion, then continues walking the result:

<script src="https://gist.github.com/w01fe/8249945.js"></script>



## Conclusion 

We've released the latest version of [Schema](https://github.com/plumatic/schema), which is both 5x faster and adds a generic facility for parallel schema-data walks.  Schema ships with an application of this facility for *coercion*, which we are using in production to automatically massage input data into a suitable form, and transparently clientize output data.  

This application is another step towards meeting Schema's design goal: enabling a single declarative definition of your data's shape that drives everything you want to do with your data, without writing a single line of traversal code.  Validation and coercion are just the first two applications, with others like test data generation just around the corner.  

In other open-source news, we're also excited to announce the `0.2.0` release of [Plumbing](https://github.com/plumatic/plumbing), which makes Graph, `fnk`, and friends Schema-friendly. Be on the lookout for more releases on the horizon, including our API definition library with validation and pluggable coercion included.  

Join the discussion on Hacker News and let us know what you think.
 


 
