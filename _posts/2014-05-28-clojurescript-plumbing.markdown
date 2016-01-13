---
layout: post
title: Plumbing Now Supports ClojureScript
date: 2014-05-28 11:10:23.000000000 -07:00
---
![](https://camo.githubusercontent.com/1fd2d0f291549208431770f5c80ffdfbee2213fd/68747470733a2f2f7261772e6769746875622e636f6d2f77696b692f707269736d617469632f706c756d62696e672f696d616765732f707269736d617469632d73776973732d61726d792d6b6e6966652e706e67)

Today we’re excited to announce that our open-source [Plumbing](https://github.com/plumatic/plumbing) library has been extended to support ClojureScript. Plumbing is a collection of Clojure utilities that we’ve found helpful regardless of what we’re working on. It contains common, general-purpose functions as well as unique tools, like [Graph](http://plumatic.github.io//graph-abstractions-for-structured-computation). It’s our most starred and watched open-source library on GitHub and has been battle tested in our fully Clojure backend. 

Similar to [Schema](https://github.com/plumatic/schema/), we are using the [cljx](https://github.com/lynaghk/cljx) library to generate both Clojure and ClojureScript source from a single codebase. With the exception of only a few JVM-specific utilities and optimizations, the entire API of Plumbing is available in ClojureScript.

Over time, we’ve found many powerful applications of Plumbing & Graph in Clojure and we’re excited to apply these to ClojureScript. However, we also suspect new and interesting ClojureScript-specific applications will arise from UI development.
