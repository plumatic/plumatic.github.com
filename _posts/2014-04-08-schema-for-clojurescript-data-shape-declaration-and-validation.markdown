---
layout: post
title: Schema for Clojure(Script) Data Shape Declaration and Validation
date: 2014-04-08 11:47:56.000000000 -07:00
---
<p><strong>tl;dr:</strong> <em>We open-sourced <a href="http://github.com/plumatic/schema">Schema</a> to get many of the benefits of type systems in Clojure with less hassle. In the future, we will use Schema declarations to do awesome things like auto-generate Objective-C classes (take a <a href="https://github.com/plumatic/schema/blob/d82bf0b049fc1205a81a79d88a84872bb9f9846b/src/clj/client/objc.clj">peek</a>). Join <a href="https://news.ycombinator.com/item?id=6334865">the discussion</a> on Hacker News and tell us what you think</em></p>

<p>One of the touted benefits of functional programming is that it produces easier to understand and more reusable code. The reasoning behind this claim goes as follows: well-written functional code consists of many small <a href="http://en.wikipedia.org/wiki/Pure_function">pure functions</a>. Since these functions are free of side-effects, you only need to understand a function's input and outputs to understand its behavior. Unfortunately, understanding what a function does and its inputs and outputs can be a non-trivial task. For instance,</p>

<script src="https://gist.github.com/aria42/c2be48407197cda0f33e.js"></script>


<p>This is a pure function. But what in the world does a <code>share-counts</code> data structure look like and what are <code>updates</code>? In this specific case, you have to actually read the body of the function and infer the nature of the inputs; in more complex real-world examples, you might not be able to tell from the function itself and have to track down calls to the function to figure out the inputs and outputs.  In a large code base, this can be a real maintainability nightmare.</p>

<p>Now, the author of this function could definitely have written a doc-string for the function, explaining the 'shape' that <code>share-counts</code> and <code>updates</code> ought to have:</p>

<script src="https://gist.github.com/aria42/4f2abff915fa97c0b282.js"></script>


<p>This is less than ideal for a number of reasons:</p>

<ul>
<li>Code often changes faster than documentation, so the doc-string will likely become outdated</li>
<li>There's no way to automatically check at compile or runtime that inputs or outputs match this specification</li>
<li>If <code>share-counts</code> data shape is used throughout the namespace, it becomes very repetitive to specify in each function</li>
<li>There are many natural language descriptions that describe same shape, making it difficult for a team to adopt uniform standards or get good quickly 'parsing' this shape description</li>
</ul>


<p>In order to easily understand this function, it's not only nice to know it's free of side-effect or mutation, we also want to understand <em>how</em> to use this function: what kind of data should I provide it and what will it give me back. Ideally, how we do this should be declarative and if we really want we can have the function check that input or output matches these expectations.</p>

<h2>Don't you really just want static types?</h2>

<p>Many readers will rightfully wonder: don't you just want your language to be statically typed?  If so, there's <a href="https://github.com/clojure/core.typed">a great Clojure project</a> out there and other JVM languages (like <a href="http://scala-lang.org">Scala</a>) for you. While having types as a first-class citizen in Clojure would address much of what we're asking for above, type systems aren't the only way to do so.</p>

<p><img src="http://cdn.meme.li/i/oc0l8.jpg" style="display:block; margin-left:auto; margin-right: auto;"></img></p>

<p>Static typing can be very useful and certainly provides documentation as well as some form of compile-time safety, but full-blown static-typing comes with many undesirable drawbacks. We don't want to rehash the static vs. dynamic typing arguments (shee <a href="http://steve-yegge.blogspot.com/2008/05/dynamic-languages-strike-back.html">here</a> for a good overview), but suffice it to say that one of the strengths of Clojure is that (almost) everything can be manipulated as data and this promotes substantial reuse when utilized correctly. A common drawback of statically-typed languages is that it often yields <em>type coupling</em>, where reuse of code becomes difficult because type unnecessarily constrains use.</p>

<p>For example, consider a Clojure function that takes a map containing a <code>:first-name</code> key and a <code>:last-name</code> key and adds a <code>:name</code> key that concatenates them. In a statically-typed language, the idiomatic way to do this might be to declare a User (case) class with a <code>firstName</code> and <code>lastName</code> fields (or have an interface for <code>Nameable</code> things) and use of the function must be limited to objects which are that class or bother to implement that interface. Conceptually though, there are lots of kinds of places in a large code base where you might want this operation (for instance, working with contact information from external networks). You don't want to limit by type, but by the attribute that your data is a map that has <code>:first-name</code> and <code>:last-name</code> keys. In Clojure(Script), you might write this as:</p>

<script src="https://gist.github.com/aria42/8576e875148fe575f9c6.js"></script>


<p>While, it's possible to get at something like this with things like <a href="http://en.wikipedia.org/wiki/Structural_type_system">structural typing</a>, it's not what type systems are optimized for, nor do they make it easy.</p>

<h2>Introducing Schema</h2>

<p>One of the issues we were running into with a large Clojure(Script) codebase was the ability to document the kind of data functions took and what they returned. Aside from documentation, when the contract of a function is broken at runtime Clojure often might not fail at all, and it's up to the developer to track a <code>nil</code> halfway through the codebase to find the root cause.</p>

<p>For these reasons, we built the <a href="http://github.com/plumatic/schema">Schema</a> library for declaring and validating data shapes. Schema isn't a full-blown type system, but a lightweight flexible DSL for describing data requirements, more approriate to how Clojure functions should delimit use. For example, our <code>with-full-name</code> example,  would be written as:</p>

<script src="https://gist.github.com/aria42/f4b91ea7fe49a2d23ce7.js"></script>


<p>The <code>:-</code> in the parameter vector says that <code>m</code> satisfies the <em>schema</em> on the right hand side; in this case that <code>m</code> is a map that has a string under the <code>:name</code> key, plus any number of arbitrary key-value mappings (the <code>s/Any</code> maps to <code>s/Any</code> part).</p>

<h3>An extensible protocol</h3>

<p>At the heart of Schema is a protocol that knows how to check if data satisfies the protocol and to explain where there is a mismatch between input data and a schema. The Schema library extends the protocol to many of the built-in data types to provide descriptions of data that read relatively naturally:</p>

<script src="https://gist.github.com/aria42/ea63d7e08d1958cc9dca.js"></script>


<h3>Schemas are still just data</h3>

<p>Since we've extended the Schema protocol to Clojure core data structures, you can think of Schemas as data that you can combine and use Clojure code to manipulate.</p>

<p>Returning to our earlier example with <code>update-share-counts</code>, many of the kinds of data passed to this function might be re-used throughout a namespace (as types are re-used in a code base). We can simply define those schemas and use them throughout our code:</p>

<script src="https://gist.github.com/aria42/b0dbc2b7131f4bb6ca60.js"></script>


<h3>Flexible validation</h3>

<p>By default, putting schemas on your functions doesn't do anything other than add documentation (which is no small thing). There are several ways to turn on validation for specific parts of your code. You can use the <code>with-fn-validation</code> macro to turn it on for any block:</p>

<script src="https://gist.github.com/w01fe/8c9bf488e9e5025726a8.js"></script>


<p>The most common use of validation will probably be as a fixture in your test namespace, so that all the functions called by your tests check schemas. In JVM Clojure and in <a href="http://clojuredocs.org/clojure_core/clojure.test/">clojure.test</a> this can be accomplished by adding</p>

<p><code>(use-fixtures :once schema.test/validate-schemata)</code></p>

<p>to the bottom of your test file.  Another great use case is to declare schematized defrecords, and use validation to ensure your data is up to snuff before persisting it or passing it over the wire.</p>

<h3>Sharing Schemas between Clojure and ClojureScript</h3>

<p>The <a href="http://github.com/plumatic/schema">Schema</a> library supports both Clojure and ClojureScript.  While Schema supports platform-specific features (like primitives in the JVM or prototype chains on JS), many schemas can actually be shared as data between the two. For instance, backend and frontend can share a set of cross-platform schemas describing the API inputs and outputs.</p>

<h2>Conclusion and Future Work</h2>

<p>We've found Schema to be a productivity win for our Clojure teams and a significant time-saver in tracking down runtime issues. We have many plans for broader use of Schema, including:</p>

<ul>
<li>Generating Objective-C domain classes from schemas (<a href="https://github.com/plumatic/schema/blob/d82bf0b049fc1205a81a79d88a84872bb9f9846b/src/clj/client/objc.clj">check out</a>  a preview of this code)</li>
<li>Generating <code>core.typed</code> form annotations from function schemas to get compile-time shape validation</li>
<li>Generating <a href="http://avro.apache.org/">avro</a> specficiations from schemas</li>
</ul>
