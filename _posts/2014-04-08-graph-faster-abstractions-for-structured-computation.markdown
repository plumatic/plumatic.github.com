---
layout: post
title: 'Graph: Faster Abstractions for Structured Computation'
date: 2014-04-08 12:09:06.000000000 -07:00
---
Join [the discussion](https://news.ycombinator.com/item?id=5640011) on Hacker News!

*This guest post is about work done by [Leon Barrett](http://leon.barrettnexus.com/), who is visiting us from our fellow
Clojure users at [The Climate Corporation](http://climate.com/company/). The Climate Corporation has "sprintbaticals," two-week sabbaticals to work on something a little different, and Leon decided to contribute to our [open-source Graph library](https://github.com/plumatic/plumbing), making it 30X faster for Climate's use case. This post is cross-posted to [their blog](http://eng.climate.com/2013/05/01/faster-graph/).*

The Promise
-----------

[The Climate Corporation](http://climate.com/company/) models the weather and how it affects the growth of crops. As one small aspect of that, they have a computation for incoming sunlight at a given latitude on a given day of the year. This code is run in the inner loop of some of Climate's models; every time they need to know how plants will fare in some place on some day, they need to compute it. They want it to be fast (microseconds per call), and of
course it should be: it consists of only several dozen floating-point operations. The code is not too complicated, just a bit of physics and trigonometry, but it is still complex enough that they benefit from the use of [Graph](https://github.com/plumatic/plumbing). In this post (spoiler alert), we're going to describe how we were able to speed up Graph by a factor of 30 for them.

As discussed in a [previous post](http://plumatic.github.io/graph-abstractions-for-structured-computation/), Graph helps reduce "[incidental complexity](http://codequarterly.com/2011/rich-hickey/)", and thus make coding easier, by letting us specify computations and their dependencies declaratively. With Graph, some things that are normally hidden in code--that the reason *this* function is called before *that* one is because its value is needed *there*--are completely explicit, making it far easier to change that code. Further, we can separately give an "execution strategy" that says how to execute a graph--in parallel, lazily, etc. One additional benefit is in the form of this programmatically-generated image, which shows the structure for a calculation of incoming sunlight.

![Image of graph of solar radiation computation](http://tccengblog.files.wordpress.com/2013/04/graph.png?w=1214&h=744)

The details of the math don't matter too much for this discussion; what does matter is that it has interesting dependencies but simple individual computations.

<script src="https://gist.github.com/leon-barrett/5497378.js"></script>

Despite all those benefits, none of the graph execution methods available in
the first version of Graph were fast enough for Climate's needs; each added
a 30X (yes, 30 *times*) overhead. Because the solar radiation functions are so
simple, any overhead in executing the graph is magnified enormously.

What was the source of this overhead? In the first version of Graph, every time
a `fnk` was called, the caller made a map (e.g. ``{:day-of-year 102 :lat 34.2}``)
and the `fnk` pulled out its parameters. So, every time we called a `fnk`, we
created a map full of its parameters, like so:

<script src="https://gist.github.com/leon-barrett/5497376.js"></script>

Similarly, the `fnk` macro generates destructuring code like this:

<script src="https://gist.github.com/w01fe/5497074.js"></script>

That use of maps was a nice win for us, making it simple to store lots
of intermediate values. We use Graph in our outer loop, so our
individual `fnk`s are much more complex (and slower) than Climate's solar
radiation calculation. In that case, the overhead of two dozen map
constructions doesn't matter. But, when applied to Climate's inner loop, those
end up being much more expensive than the several dozen floating-point
operations.

Code generation is your friend
------------------------------

Rather than do map construction and destructuring with every function call, it would be more efficient to, well, just call a function with the desired parameters. It's easy to have the `fnk` macro generate a normal positional function. Then we just need to arrange to call that function with the right arguments in the right place, making something like this:

<script src="https://gist.github.com/w01fe/5497079.js"></script>

However, notice that we'll need to generate this code at runtime. The structure of a graph may not be known at compile time; we might want to ``assoc`` new `fnk`s into a graph or ``select-keys`` to pull an important piece out of the graph. That means macros will not be enough; even though famously "[eval is evil](http://blogs.msdn.com/b/ericlippert/archive/2003/11/01/53329.aspx)", we need to eval some code. But eval isn't evil in Clojure--there's no heavyweight construction of a compiler, no risk of mis-interpreting strings, just the same machinery we use to write macros. So we can inspect the graph at runtime, generate a nice little let, and then eval it, rather like this:

<script src="https://gist.github.com/leon-barrett/5497364.js"></script>

(For even more speed, we define a record for the output of the graph. A record is a lot like a map with a fixed set of keys, backed by a class with a member datum for each key, so it's as fast as the JVM allows.)

The result is that Graph can now compile our tiny arithmetic graph with not 30X, but rather 20% overhead, essentially just the cost of calling twelve different functions (and a little for checking of optional parameters). Nothing is lost, and everything is backwards compatible--the new positional functions are stored in the `fnk`'s metadata, and the earlier graph compilation methods are still available. But now if you need to exercise your floating-point unit, Graph is prepared to help.

To inspect exactly what was changed, you can see the change on [Github](https://github.com/plumatic/plumbing/blob/v0.1.0/CHANGELOG.md).  Add `[prismatic/plumbing "0.1.0"]` to your Leiningen project to try it out.
