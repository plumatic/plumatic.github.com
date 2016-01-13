---
layout: post
title: 'Schema 1.0: (generate (s/conditional witty? Title))'
date: 2015-08-31 11:04:22.000000000 -07:00
---
TL;DR: Schema graduates from alpha with a simpler and faster backend, as well as support for automated test data generation from Schemas. 

[Schema](https://github.com/plumatic/schema) is a Clojure library for declaring and validating the shape of data. Schemas themselves are simple, declarative Clojure data structures, which allows them to be used in many different applications. Across the Clojure community, schemas are already heavily used to validate data, and coerce data to fit a desired form. With the release of Schema 1.0, the schema internals have been completely reworked to make it flexible and easy to add entirely new applications. With this release, the library adds support for generating data from a schema, and completion, which fills out missing parts of a datum to match a schema.

## Schema

Schemas provide an intuitive way to describe the form of data. To get a sense for Schema, let’s look at an example:

<script src="https://gist.github.com/davegolland/624d36505b186da8c9bb.js"></script>

This schema describes a simple bank account represented as a map that has an account number, a type (either `:checking`, or `:savings`), an owner with a name (string) and an age (positive integer). The account also has a sequence of transactions, each represented by a map that has an amount (double) and an optional memo (string).

Here is an example of a bank account that will match this schema:

<script src="https://gist.github.com/davegolland/12c338cd08882409753b.js"></script>

Don’t take my word for it, we can programmatically verify that this account matches the schema:

```
(s/validate Account turing)
;; success!
```

And we can detect malformed accounts:

<script src="https://gist.github.com/davegolland/f7ebc2ea2055ac0a6269.js"></script>

Note that Schema produces legible error messages that describe exactly what is wrong with the data it is validating. For dynamically typed languages like Clojure, having a concise way to validate data is extremely useful for catching errors without writing lots of boilerplate validation code.

By describing data in a declarative way, schemas can be used for much more than validation. 

### Completion

Schemas can be very useful when testing code.

Many functions may operate over complex data types, but only care about a small part of their inputs; the rest of the data in the inputs is irrelevant. However, when creating a proper test case, you end up having to provide complete values, even for the irrelevant parts of the input. For example, let’s say that we had a function that operates over Accounts such as the following:

<script src="https://gist.github.com/davegolland/865709c3c4a7cb027c07.js"></script>

We can test that this function is correct by running it on a specific account:

```
(is (= -46.65 (account-balance turing)))
```

However, this is just one example. To boost confidence, we would ideally like to test more than just one input. The `account-balance` function only really depends on the transactions, so it seems unnecessary to have to specify the rest of the fields in the Account map (e.g. `:owner`, `:type`, etc). With schema completers, we don’t have to:

<script src="https://gist.github.com/davegolland/b29acd38ddaaa86f18f4.js"></script>

With completers, we just need to specify the relevant parts of the input (the transactions), and the completer fills in the rest of the data.

<script src="https://gist.github.com/davegolland/41ed1e4f767be22b5b1c.js"></script>

Note that the completer preserved the `:transactions` field, but populated the other fields with dummy values that match the schema: the account type is one of the enum values (i.e. `:checking`) and the `:age` is a positive integer.

### Generative Testing

Generative testing is a technique that automatically generates input data for testing the behavior of a function. Programmers typically enjoy using generative testing because it automates the task of manually creating test cases and allows them to develop code faster and focus on a higher level of abstraction. Moreover, letting a system produce test cases typically results in more thorough coverage because many more inputs are tested. 

Generative testing of a function proceeds by first identifying a property of the function that is invariant across inputs, creating a generator that produces test data of the right type, and finally verifying that the property holds for all of the generated data. Many generative testing libraries come with generators for primitive types (e.g. strings and integers), but they leave the task of writing generators for user-defined types to the programmer. Unfortunately, the effort needed to compose the primitive generators for each user-defined type can discourage programmers from using generative testing.

Fortunately, each Schema lends itself well to automatic processing: the schema generators library can automatically construct a generator for most schemas. Returning to our `Account` example from above, we can verify that the `account-balance` function works over many different values:

<script src="https://gist.github.com/davegolland/8e65e9f7e87270fa9d5c.js"></script>

Here we’re testing an alternative implementation of the `account-balance` (in practice you could use a slower, reference implementation in this spot) and verifying that it matches our function for 100 different values.

By default, Schema’s generating library uses reasonable defaults for the primitive generators, but it also provides way to plug in custom generators.

## Motivation and Internals

### On Extensibility 

Schema was designed to be easily extensible: users can add new schemas in client code without modifying the library. When these schemas are added, they automatically can be used in composite schemas and in the various schema applications.

Prior to Schema 1.0, adding new applications (such as completion or generation) was difficult. Each schema type had to be manually updated to support the new application, and any private schema types unknown to the application developer would not benefit from the new functionality. On the flipside, each new schema that is added would need to implement each application. In short, we had an NxM problem: all of the N schema types need to support each of the M applications -- often with a fair bit of repeated boilerplate. This scaling factor discourages developers from introducing new schema types and especially discourages them from building new schema applications.

### Leaf, Variant, and Collection Specs

>"All problems in computer science can be solved by another level of indirection."
>
>-- Butler Lampson 

To solve our NxM problem, we decided to group similar schema types together under the same "spec" -- applications now only need to support three different specs, rather than a large, open set of schemas. As an example, you can check out the implementation for [generators](https://github.com/plumatic/schema/blob/master/src/clj/schema/experimental/generators.clj). There are composite schemas that are defined in terms of other schemas, and **leaf** schemas that are not (e.g. int, string, regex). The composite schemas further divide into **collection** schemas, where multiple schemas coexist as sub-schemas of a larger collection (e.g. map, seq), and **variant** schemas, where a single schema is defined in terms of mutually exclusive sub-schemas (e.g. maybe and conditional). For the curious, there is more information about [defining new schema types](https://github.com/plumatic/schema/wiki/Defining-New-Schema-Types-1.0). Introducing the leaf, collection, and variant specs adds a layer of indirection that reduces our NxM problem to (N + 3M): each of the N schemas just need to implement one of the specs, and each of the M applications just needs to support the 3 different types of schemas.

A typical rejoinder to Lampson’s quote is: "*Any performance problem can be solved by removing a layer of indirection."* Witty rejoinders notwithstanding, Schema 1.0 is twice as fast as its predecessor. Consolidating the application logic to work over schema specs means that it’s sensible for us to pull out all the stops in optimization. New schemas implementing these specs simply benefit from these optimizations, the work doesn’t need to be duplicated across all schemas. Furthermore, it's much easier to new implementations of the complex collection schemas, not just because they automatically benefit from multiple applications, but because the spec abstraction layer already understands how to validate a collection.

## Conclusion

This application is another step towards meeting Schema's design goal: enabling a single declarative definition of your data's shape that drives everything you want to do with your data, without writing a single line of traversal code. This release adds test data generation and completion to the growing list of applications, while also improving performance and simplifying the APIs for extending Schema or providing Schema-related tooling. 

Join the [discussion](https://news.ycombinator.com/item?id=10154400) on Hacker News and let us know what you think.

