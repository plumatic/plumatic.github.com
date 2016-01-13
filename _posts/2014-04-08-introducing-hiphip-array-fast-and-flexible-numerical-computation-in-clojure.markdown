---
layout: post
title: 'Introducing HipHip (Array):  Fast and flexible numerical computation in Clojure'
date: 2014-04-08 12:01:22.000000000 -07:00
---
Join [the discussion](https://news.ycombinator.com/item?id=6021053) on HackerNews!

_This post and work on HipHip are done in part by [Leon
Barrett](http://leon.barrettnexus.com/), who is visiting on a 'sprintbatical' from fellow Clojure shop [The Climate Corporation](http://climate.com/), and [Emil Flakk](http://flakk.me). This post is cross-posted to [The Climate Corporation's blog](http://eng.climate.com/2013/07/10/introducing-hiphip-array-fast-and-flexible-numerical-computation-in-clojure/)._

Climate Corp. shares the unique challenge of developing new algorithms and making them work at a large scale. This combination of research and engineering takes many cycles of algorithm and model design, including rapid implementation to test on lots of real data. In contrast to most engineering prototyping,  such numeric modeling demands high performance out of the gate, since correctness cannot be judged on just a little bit of data. In this post, we introduce our open-source array processing library [HipHip](https://github.com/plumatic/hiphip), which combines Clojure's expressiveness with the fastest math Java has to offer. 

# Computational Functional Programming

As we've outlined in [past blog posts](http://plumatic.github.io/graph-abstractions-for-structured-computation/), we love Clojure (and [functional programming](https://en.wikipedia.org/wiki/Functional_programming) generally) because it allows us to quickly build performant and reusable tools. However, many of the data abstractions that make Clojure code so reusable don't yield fast numerical computation. For instance, consider the [dot product](http://en.wikipedia.org/wiki/Dot_product) operation, which is the core performance bottleneck in most machine learning algorithms. Using Clojure's built-in sequence operations, it would be natural to write the dense dot product as:

<script src="https://gist.github.com/w01fe/5963969.js"></script>
<img src="http://i.qkme.me/3v3uat.jpg" style="display:block; margin-left:auto; margin-right: auto;"></img>



The above code succinctly expresses the idea that we form the sequence of element-wise products between `ws` and `xs`, and then we sum those entries.  But its performance is a disaster, especially if you're considering real-world machine learning use where each prediction could require thousands of dot products. The core of the slowness arises from the fact that intermediate operations produce sequences of 'boxed' Java [Double](http://docs.oracle.com/javase/6/docs/api/java/lang/Double.html) objects. All arithmetic operations on these boxed objects are significantly slower than on their primitive counterparts. This implementation also creates an unnecessary intermediate sequence (the result of the `map`), rather than just summing the numbers directly.

# Clojure's built-in array macros vs. plain-old Java

Clojure comes with built-in array processing macros ([`amap`](http://clojuredocs.org/clojure_core/clojure.core/amap) and [`areduce`](http://clojuredocs.org/clojure_core/clojure.core/areduce)) which extend Clojure sequence operations to arrays and allow for primitive arithmetic.  Here's what dot-product looks like using those macros:

<script src="https://gist.github.com/w01fe/5963975.js"></script>
<img src="http://cdn.memegenerator.net/instances/400x/31063019.jpg" width="300" height="300" style="display:block; margin-left:auto; margin-right: auto;"></img>


The performance here is acceptable, but the code itself is pretty convoluted.  Arguably, this isn't even much of an improvement over the Java version:

<script src="https://gist.github.com/w01fe/5963980.js"></script>

# Why not just use Java for math bottlenecks?

<img src="http://media.fakeposters.com/results/2009/07/17/21yzgn6j9o.jpg" width="300" height="240" style="display:block; margin-left:auto; margin-right: auto;"></img>


Since the dot product is a crucial bottleneck for machine learning, why not just suck it up and use the Java version? Clojure makes Java-interop easy so that you can take the core bottleneck of your system and make it fast.

The problem is that for computational systems, there typically isn't a single bottleneck. Instead, any function that performs array operations (whether  `double`, `long`, `float`, etc.) over each item of data can cause an unacceptable slowdown, even during the research prototyping stage. Since these sorts of array operations are the bulk of our computational code, our research engineers would mostly be stuck writing Java. We need the expressiveness of Clojure **and** the speed of Java primitives.

# Introducing the HipHip array processing library

<img src="http://i.qkme.me/3v4p30.jpg" style="display:block; margin-left:auto; margin-right: auto;"></img>


We're happy to announce [HipHip](https://github.com/plumatic/hiphip), a Clojure array-processing library. Here's what a dot product looks like in HipHip:

<script src="https://gist.github.com/w01fe/5963993.js"></script>

All the arithmetic operations are performed over primitive doubles and which allows the speed to match Java. Here we get a clean, functional representation of the operation that matches Java performance without any worrying about type hints.

HipHip provides namespaces for working on arrays of the four main primitive math types (`double`, `float`, `long`, and `int`) without type hints, which include:

 * Versions of `clojure.core` array fns that do not require type hints:

<script src="https://gist.github.com/w01fe/5963995.js"></script>

 * A family of macros for efficiently iterating over array(s) with a common binding syntax, including 
 `amake`, `doarr`, `afill!`, and our own versions of `amap` and `areduce`.

<script src="https://gist.github.com/w01fe/5963999.js"></script>
  
 * Common 'mathy' operations like summing over an array and selecting a maximal element (or many): 

<script src="https://gist.github.com/w01fe/5964019.js"></script>

You might find some of these operations useful, even if your data isn't already in arrays.  For
example, the following function can find the top 1000 elements of vector `xs` by `score-fn`
about 20 times faster than `(take 1000 (reverse (sort-by score-fn xs)))` on 100k elements.

<script src="https://gist.github.com/w01fe/5964022.js"></script>

HipHip also provides a generic namespace `hiphip.array` with variants of the iteration macros
that work on all (hinted) array types, including mixing-and-matching a variety of array types 
in the same operation:

<script src="https://gist.github.com/w01fe/5964025.js"></script>

HipHip uses [Criterium](https://github.com/hugoduncan/criterium) for benchmarking, and runs benchmarks as tests, so we can be pretty confident that most of our double array operations run 0-50% slower than Java (but we're not quite there for other array types yet, see the 'Known Issues' section of the readme for details).  The readme also describes a critical option for fast array math if you're running under Leiningen.

[Check it out](http://www.github.com/plumatic/hiphip) and let us know what you think in the [Hacker News](https://news.ycombinator.com/item?id=6021053) thread.
 
