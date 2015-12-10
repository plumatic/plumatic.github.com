---
layout: post
title: Faster, Better DOM manipulation with Dommy and ClojureScript
date: 2014-04-08 12:10:14.000000000 -07:00
---
  
  
<p>Last year, we migrated our web development to <a href="https://github.com/clojure/clojurescript">ClojureScript</a> and haven&#8217;t looked back. Over the last few months, we&#8217;ve been building out <a href="https://github.com/prismatic/dommy">dommy</a>, a ClojureScript library for DOM templating and manipulation. The library is relatively far along and we think it’s time to share our reasons for why we’d write yet another DOM library, rather than <a href="https://github.com/ibdknox/jayq">use jQuery within ClojureScript</a>. We built dommy because we felt that a ClojureScript DOM library could be a simpler, faster, and better version of jQuery that fits organically into expressive functional code. In this post, we&#8217;ll talk about how we made dommy so much faster than jQuery (for example, 15x faster for a simple ID selector) and how we can acheive jQuery-style patterns with less overhead using ClojureScript-native abstractions.</p>

<h1>Macros make you much faster</h1>

<p>ClojureScript has a <a href="http://blog.fogus.me/2012/04/25/the-clojurescript-compilation-pipeline/">compilation step</a> which converts Clojure into JavaScript. During compilation, ClojureScript macros examine your code and can re-write expressions that generate more efficient JavaScript upfront. Macros enable the much of the work the client does at run-time to be moved to compile-time. The differences can be pretty dramatic for common web development tasks, particularly those in performance-sensitive environments like mobile.</p>

<h2>Selectors</h2>

<p>Probably the most common uses of jQuery are DOM selectors. Most selectors in the wild are simple queries for either an element by ID (e.g., <code>$(‘#some-id’)</code>) or by class (e.g, <code>$(‘.some-class’)</code>). Both jQuery and dommy eventually delegate to fast native DOM methods for such selectors. However in jQuery, the work of parsing the selector string and detecting that is a simple ID selector is done on the client runtime. Dommy, on the other hand, parses the expression at compile-time and rewrites the selector directly to the native DOM method where applicable. Here are some specific examples of how selectors are rewritten:</p>


<script src="https://gist.github.com/aria42/26d12fdfa19ddb7e709f.js"></script></p>

<p>jQuery, by contrast, <a href="https://github.com/Prismatic/dommy/blob/master/test/dommy/core_test.cljs#L376">incurs significant overhead</a> in parsing the selector at runtime:</p>

<script src="https://gist.github.com/aria42/aabd4a18f4f33844f41a.js"></script></p>

<p>While JavaScript as a compilation target has become much more popular with libraries (<a href="https://developers.google.com/closure/">Google Closure</a>) and languages (<a href="http://coffeescript.org/">CoffeScript</a> and <a href="https://code.google.com/p/dart/">Dart</a>), none of these options have the power of macros to allow clients to control the compilation process. In fact, few languages outside of the LISP family (which includes Clojure) have access to macros for optimizations such as this. Macros are useful for more than just selectors, they also come in handy for another common DOM task: templating.</p>

<h2>Templating</h2>

<p>We covered dommy macro templating in <a href="http://blog.fogus.me/2012/04/25/the-clojurescript-compilation-pipeline/">a past post</a>, but it bears repeating since it&#8217;s yet another example of why macros are awesome for client-side performance. Suppose you want to generate DOM for a user profile where each profile displays several user-generated posts. In dommy you might create two small templates for this:</p>

<script src="https://gist.github.com/aria42/7fec57a40de180a741cb.js"></script></p>

<p>This nested data in this template is compiled straight to efficient Javascript DOM method executions (<a href="https://gist.github.com/aria42/f2ad8ff911364c6d3dc0">see here</a>). The jQuery equivalent is much more unwieldy and requires separating out part of the template, because it has to be dynamically generated. It is also <strong>2x slower than dommy</strong> (<a href="https://gist.github.com/aria42/91988c4e5a67c460dd2a">see test here</a>):</p>

<p>
<script src="https://gist.github.com/aria42/da6def0276374dfdc12b.js"></script></p>

<p>Again the time saving comes from macros moving the work of parsing the template data structures from runtime to compile-time and directly generating efficient JavaScript. </p>

<h1>Simpler, Better, and Extensible</h1>

<p>jQuery has many powerful patterns such as method chaining and being able to seamlesslesly handle either a single or multiple elements. Part of why we wrote dommy was because we could acheive the same level of expressiveness in ClojureScript without the complications and overhead jQuery undergoes to support these features in JavaScript.</p>

<h2>Away with DOM wrapper objects</h2>

<p>jQuery outputs are typically DOM objects inside a wrapper object. A source of frustration in jQuery is not being sure if you’re dealing with an actual DOM element or jQuery’s wrapper around such an element. There is also a non-trivial performance hit. So what’s the upshot? The jQuery object effectively exists to support chaining of jQuery methods:
<script src="https://gist.github.com/aria42/097053326ee048c9a2f8.js"></script>
  
  In dommy, each of our DOM manipulation functions return plain unwrapped DOM nodes. We use Clojure&#8217;s single-threaded arrow macro <a href="http://blog.fogus.me/2009/09/04/understanding-the-clojure-macro/"><code>-&gt;</code></a> to chain expressions:
<script src="https://gist.github.com/aria42/b8fdc22902529fc2ecd6.js"></script></p>

<p>In Clojure, the idea of threading an object through a series of functions which update the object and return the updated object can be expressed cleanly using the <code>-&gt;</code> macro. To acheive this pattern in JavaScript, jQuery has to use wrapper object methods (like <code>addClass</code>) instead of just a function to support chaining. There isn&#8217;t a better option in JavaScript, but there is in Clojure and dommy leverages that.</p>

<h2>One is the loneliest number</h2>

<p>Another messy part of jQuery&#8217;s semantics are that many (but not all) of the jQuery methods will happily work on one or many things which can be coerced into jQuery DOM objects. The intent is to support processing one or many objects in a similar way. Here&#8217;s a concrete example in jQuery:</p>

<p><script src="https://gist.github.com/aria42/476e00b04c35aec1d09d.js"></script></p>

<p>jQuery&#8217;s API has a suite of sequence processing function (<code>filter</code>, <code>sort</code>, etc.) which don&#8217;t conceptually belong in jQuery, but are there to compensate for the widespread lack of such in JavaScript. But in order to do the sequence processing, the poor <code>addClass</code> function is forced to operate on both a singleton and a sequence. </p>

<p>In dommy, <code>add-class!</code> only works on a single DOM object, but we still the sequence-processing expressiveness using built-in Clojure language features:</p>

<p><script src="https://gist.github.com/aria42/6b73de43fa99cce07697.js"></script></p>

<p>We can even use Clojure’s standard library to do things you can’t easily do with jQuery:</p>

<script src="https://gist.github.com/aria42/b0d08f352980cb3f2f37.js"></script></p>

<p>Aside from just yielding a smaller library, dommy&#8217;s avoidance of wrapper objects is also a significant performance boost since no runtime cycles are wasted trying to guess what your input objects are and constructing objects to wrap them.</p>

<h2>Extensible</h2>

<p>Like jQuery, we want our DOM manipulation to accept multiple kinds of things which can be coerced to DOM-like objects. In jQuery, this is limited to a pre-defined set of things (strings representing html, native DOM objects, and jQuery wrapper objects themselves). In dommy, there is a <a href="http://clojure.org/protocols">Clojure protocol</a> for what can be converted to a DOM, which any object can implement. We extend that protocol to native Clojure data structures (this is how templating works in dommy). This makes it really easy to extend the DOM manipulation and templating system in client code:</p>

<script src="https://gist.github.com/aria42/808b4daa94efc7168d77.js"></script></p>

<p>Altogether, dommy’s integration into the Clojure environment lets you treat DOM nodes as first-class objects and protocols:</p>

<script src="https://gist.github.com/aria42/75aaddcae8b14f40a246.js"></script></p>

<h1>Are you saying jQuery sucks?</h1>

<p><b>No</b>. jQuery was a massive improvement in web development, but was fundamentally limited by the language that it was implemented in. Dommy is mostly us re-considering how we&#8217;d write DOM manipulation and templating in a way that felt Clojure-y and functional native, drawing on a lot of the good choices jQuery did make. </p>

<p>In fairness, jQuery currently has more features and supports a much broader range of browsers. Over the next few months, we plan on increasing dommy&#8217;s coverage to cover at least 95% of browsers (somewhere on par with jQuery 2.0&#8217;s coverage). Stay tuned for future developments and updates to dommy!</p>
</body>
</html>
