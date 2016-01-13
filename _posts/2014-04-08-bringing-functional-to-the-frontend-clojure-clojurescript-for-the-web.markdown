---
layout: post
title: 'Bringing functional to the frontend: Clojure + ClojureScript for the web'
date: 2014-04-08 12:13:03.000000000 -07:00
---
Earlier last year, we gave a [talk](http://www.infoq.com/presentations/Why-Prismatic-Goes-Faster-With-Clojure) about how we use [Clojure](http://clojure.org) to craft fine grained software abstractions for maximal productivity and reuse. On the back-end, Clojure is a great fit for our functional programming approach to software engineering. On the front-end however, none of the popular languages (Objective-C, JavaScript, Java) or frameworks are really functional, making it harder to design and test good engineering abstractions. Part of our new year's resolution was to bring the same level of engineering and abstraction to our front-end applications as we do our back-end services. We've started by migrating ouar web application away from [Node.js](http://nodejs.org) and JavaScript to Clojure and [ClojureScript](https://github.com/clojure/clojurescript). The resulting code is smaller, more expressive, and has allowed us to introduce many optimizations. 

## Migrating from Node to Clojure

The first version of our web application used a pretty standard web stack: [ExpressJS](http://expressjs.com/) as a server running on [Node.js](http://nodejs.org/), [Jade](http://jade-lang.com/) as a server-side templating language and [Jadevu](https://github.com/LearnBoost/jadevu) for client-side templating (and [Stylus](http://learnboost.github.com/stylus/) for CSS, which will be the subject of a future post). 

Here's an example of what functional programming and abstractions can buy you in a web server. Much of the code in a web server is concerned with templating data to HTML. In Node.js land, there are many templating options, but most are derived from [Haml](http://haml.info/) and define their own language which compiles to JavaScript (or directly to HTML). Here's what templating looked like in our old template engine Jade:


<script src="https://gist.github.com/4527241.js"></script>

It more or less looks like HTML but opts to use indentation rather than angle-bracket tags for structure. As a nice addition, it allows the use of css-style selectors (e.g. <code>a#id.class_name</code>). You can also inject JavaScript in certain ways to have logic in your templates (which isn't a great idea, but hard to avoid for larger structures). For instance, we wrap certain asset paths with a <code>cdnAsset</code> function to convert those paths into urls that use our CDN rather than being requested directly from the server.

The largest benefit of this style of templating is that you get a relatively clean syntax, but this comes at the cost of adding another language to your arsenal which is less expressive than the one you started out with (JavaScript). When we migrated our web server to Clojure, we opted to instead use native Clojure data structures to express HTML structure using the wonderful [Hiccup](https://github.com/weavejester/hiccup) library:

<script src="https://gist.github.com/4527247.js"></script>

The above is valid Clojure code, not another language whose syntax you have to learn or separate compiler to use. It's the same language you're using to write the core logic of your web server. You trade the whitespace oriented syntax for a data structure oriented syntax, but you gain a lot since you have a full and powerful language at your disposal:

   * You can write convenience functions to remove boilerplate more effectively, for instance the <code>include-css</code> and <code>include-javascript</code> functions are simply generating the data for anchor and CSS stylesheet elements (You could in principle write partial jade templates for these, but for a single element they wouldn't really make things more concise).
   * You can avoid the procedural for-loop to add scripts.  Instead, just use map to generate the data for scripts, which is both less code and more functional.
   * Since your template is data, you can write 'middleware'-like functions to transform template data.
   
As an example of this last point, you might notice the absence of <code>cdnAsset</code> in the Clojure template above. Wrapping every relative asset URL in this function is a bad idea; it is tedious and error prone. Since our Clojure templates are just Clojure data, we can instead transform the data before converting it to HTML to map each resource link to it's CDN variant:

<script src="https://gist.github.com/4527372.js"></script>

This makes it far easier to ensure that all the assets we want in the CDN are there without having to remember in each of our templates. 

What we describe above is possible in Jade or other templating engines, but doing it in Clojure allows for more re-use of existing code and avoids bloating our app with a special-purpose templating language to learn and configure. In general, we always prefer to use a full programming language and avoid special purpose scripting or templating languages.  You could in principle write something like Hiccup in Javascript, but the lack of keywords and arsenal of functions to manipulate data would make it less than ideal.


## Bringing it to the client: ClojureScript 

When [ClojureScript](https://github.com/clojure/clojurescript) was announced about a year and a half ago, we were excited to bring a lot of what we love about Clojure to client-side web code. Unfortunately, ClojureScript had the issues you would expect a brand new language to have: scant to nonexistent documentation, poor performance, bugs, and a relative absence of development tools for building and debugging apps. To give an example of an early issue: persistent maps were implemented by copying the entire map every time you added a key-value pair. For these reasons, we decided to stick with straight JavaScript. 

Our migration to Clojure for our web server afforded us the chance to re-examine ClojureScript. We're happy to say that ClojureScript has matured substantially since we first looked at it and we are now running it in our production web app. The first thing we moved to ClojureScript was our client-side templating to do things like render articles and search results on the client from back-end JSON data.

### Introducing [dommy](http://github.com/plumatic/dommy): Fast Clojurescript Templating

We made a similar transition in client-side templating as on the server: we replaced
[jadevu](https://github.com/LearnBoost/jadevu) with a ClojureScript-native approach very similar to Hiccup. We tried some popular ClojureScript templating libraries (e.g., [crate](https://github.com/ibdknox/crate)), but couldn't find one with acceptable performance, so we wrote one -- and we're happy to announce that this library, [dommy](http://github.com/plumatic/dommy), is open-source. The entire templating library is [one small file](https://github.com/plumatic/dommy/blob/master/src/dommy/template.cljs).

### But what about performance?

One potential concern with using ClojureScript is that you're taking a large performance hit relative to something more standard like jQuery. To address this concern, we compared performance of three client-side templating approaches: jQuery, [crate](https://github.com/ibdknox/crate), and our library [dommy](https://github.com/plumatic/dommy); see our [pert test](https://github.com/plumatic/dommy/blob/master/test/dommy/template_perf_test.cljs) for the code. The task was to generate 10,000 nested structure elements and add them to a DOM parent. Here's the ClojureScript representation of the DOM element in dommy (the crate representation is almost identical):

<script src="https://gist.github.com/4527612.js"></script>


For comparison, this is what the templating would look like using jQuery in JavaScript:


<script src="https://gist.github.com/999d4e393d612b4fbfd4.js"></script>

Of course, we could use something like [jQuery templating](http://net.tutsplus.com/tutorials/javascript-ajax/quick-tip-an-introduction-to-jquery-templating/) or another client-side  JS template library, but our only goal in comparing against jQuery is to see how much performance we are losing relative to Clojurescript templating.

Here are the number of seconds it took to make 10,000 elements averaged over 3 trials:

**jQuery**: 1.57 secs <br>
**dommy**:  2.17 secs <br>
**crate**:  6.97 secs <br>

So our dommy ClojureScript templating is only roughly 38% slower than jQuery, but 300% faster than crate. With some tuning, we were able to get the expressiveness of Clojurescript in templating and pay relatively little cost above jQuery. Overall, we feel the expressiveness of ClojureScript templating with dommy is well worth the minor loss of efficiency.

### Go Use ClojureScript!

[ClojureScript](https://github.com/clojure/clojurescript) has matured into a production-ready language that's already improved our web application code. It allows us to build better, more reusable, client-side web code in less time and with fewer lines than native Javascript. It merits serious consideration by anyone building sophisticated web applications. 
