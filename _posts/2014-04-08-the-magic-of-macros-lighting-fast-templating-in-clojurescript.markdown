---
layout: post
title: 'The Magic of Macros: Lighting-Fast Templating in ClojureScript'
date: 2014-04-08 12:12:02.000000000 -07:00
---
Last week, we [wrote](http://plumatic.github.io//bringing-functional-to-the-frontend-clojure-clojurescript-for-the-web) about transitioning our web application from Javascript to Clojure and ClojureScript. In that post, we introduced, [dommy](http://github.com/plumatic/dommy), a ClojureScript DOM templating library which expresses DOM structure using  nested Clojure data structures. Here's a simple example:


<script src="https://gist.github.com/4527612.js"></script>

This post is about how we used ClojureScript macros to transform the above data structure at compile-time into extremely efficient JavaScript
which is over **468% faster** than before and over **300%** faster than our procedural jQuery baseline:


<script src="https://gist.github.com/999d4e393d612b4fbfd4.js"></script>

Here are the performance numbers using the same [performance test](https://github.com/plumatic/dommy/blob/master/test/dommy/template_perf_test.cljs) from our last post:

**[jQuery](http://jquery.com/)**: 1.57 secs 

**[dommy](https://github.com/plumatic/dommy)**: 2.06 secs 

**[crate](https://github.com/ibdknox/crate)**: 6.97 secs 

**[dommy-macro](https://github.com/plumatic/dommy/blob/master/src/dommy/template_compile.clj)**: 0.44 secs

Here, dommy-macro is using our new macro compilation. The Clojure code for dommy macros can be found [here](https://github.com/plumatic/dommy/blob/master/src/dommy/template_compile.clj) in one small file. Dommy without macros is about 10% better than the number we reported in our previous post thanks to some [great](https://github.com/plumatic/dommy/commit/8a3d6094c56a9a1b4644fac846be09f98f84c3dd) [commits](https://github.com/plumatic/dommy/commit/16f285bacaf96a7ffbcb8c31d3375adf69788399) from the GitHub community. 

The rest of this post will explain ClojureScript macros (and macros more broadly) and how we used them to do most of the templating work at compilation time.

## Runtime Blues: Haven't I done this before? 

So how in the world did we get ClojureScript to be so quick? Well, let's take a closer look at a simple example. Consider this really simple template:

<script src="https://gist.github.com/97618c790eb656888067.js"></script>

It's clear that we could have also written this function in JavaScript as:

<script src="https://gist.github.com/edb3c1bdf9042f07ecd0.js"></script>

The problem is that when <code>simple-template</code> executes as a ClojureScript function, it's actually generating **much** more code. First, the vector above translates to JavaScript which builds the vector data structure:


<script src="https://gist.github.com/93b5fb2721dbf30fa283.js"></script>

Then this data structure is passed to a function which generates a DOM node by walking over the vector.  It has to take the <code>:a.anchor.span</code> keyword and parse it to separate the <code>a</code> element tag from the CSS classes (<code>anchor</code> and <code>silly</code>). Then it iterates through the attribute map argument to set DOM element attributes. Finally, it looks at the last element which is a string and creates a TextNode and appends it to the anchor element. 


This is crazy, right? This work is done on each execution of the function and it never **remembers** that the structure it's building is always the same. In fact the only thing that needs to change each time is the string that it appends to the anchor element. All the other work, it really needs to do just once. 

Macros allow you to do that repeated work once when you compile this code in Javascript.

## ClojureScript Macromagic

A macro is simply a function which executes at compile time and produces more code. Macros are particularly nice in LISP languages since code is data (the fun to pronounce [homoiconicity](http://en.wikipedia.org/wiki/Homoiconicity) property) and so  writing a function which produces code is not very different from a function which makes data (i.e., a normal function). 

Macros in ClojureScript are especially odd, since the ClojureScript compiler is JVM Clojure program that produces JavaScript, which in turn runs in a JavaScript VM. A  ClojureScript macro is actually just normal Clojure code which generates ClojureScript code. 

As we noted in the last section, most of the work in processing <code>simple-template</code> can be done once at compile time. Here's a very simple macro that generates DOM elements corresponding to a single vector. This code **won't** work on a broad range of inputs, but is only illustrating how this kind of macro works:

<script src="https://gist.github.com/653291dc18191fc73e61.js"></script>

So there's a lot going on here to process, but the basic idea is that the return of this function is the <code>`(let …)</code> expression which represents the more efficient ClojureScript code. At compile time, let's say you execute <code>(compile-simple-element [:a {:href \"http://somelink\"} \"Hello\"])</code>. This produces the following ClojureScript code:

<script src="https://gist.github.com/c40ed316812414619a50.js"></script>

Effectively, the macro splices in some of the arguments into the form of the macro.  The ClojureScript compiler in turn converts this to the following JavaScript:

<script src="https://gist.github.com/4224fdf23bec6af1278b.js"></script>

Obviously, the above macro doesn't handle many cases like the CSS-style element names (<code>:a.span.anchor</code>), but the actual working version of the macro has all of Clojure at it's disposal to parse the keyword and add these bells and whistles. 


### Macro Possibilities 

Macros are surprisingly powerful when used in the right way. They can transform succinct ClojureScript template code into highly efficient JavaScript, yielding much faster performance than popular native JS frameworks (like jQuery). In a future post, we'll talk about our flop Clojure library, which uses macros to express floating point operations over arrays. This lets us do extremely fast array math which is performant but still maintains Clojure's succinctness. There are plenty of other great related macro applications including DOM selection and manipulation, but we'll save that for a future post. 



