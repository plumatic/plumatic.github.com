---
layout: post
title: Clustering Related Stories
date: 2014-04-08 13:04:34.000000000 -07:00
---
The funny thing about feature requests is how in sync they are. Right now, users are clamoring for the ability to blacklist topics and publishers (coming soon!), but before that many users complained that different stories about the same event were dominating their feeds (think the recent iPad 3 launch or the Instagram/Facebook acquisition). In this post, I’m going to describe the system we built to cluster related stories which is fast, accurate, and scalable. Although I’ve spent years building state-of-the-art natural language processing (NLP) software in [grad school](http://nlp.stanford.edu/jrfinkel/), Prismatic is my first “real” job. Along the way, I’m going to try to convey what I’ve learned about building NLP and machine learning systems in the wild.

## It's an easy task for humans

Let’s start by trying to narrow in on the problem we’re trying to solve. We’re going to say that two articles are in the same event cluster if they main contents of the articles are describing the same real-world event. So for instance,

* [Steve Jobs movie starring Kutcher to focus on CEO's early years](http://www.cnet.com/news/steve-jobs-movie-starring-kutcher-to-focus-on-ceos-early-years/)
* [Steve Jobs biopic with Ashton Kutcher will weirdly not cover the iPod & iPhone eras](http://venturebeat.com/2012/04/15/steve-jobs-biopic-ashton-kutcher-no-iphone/)

Are both describing the same horrific real world event and should be clustered. Whereas,

* Steve Jobs Was Right to ‘Steal,’ and Beer is Inspiring

is also about Steve Jobs and Apple, but not describing the same event as the other two. Let’s start by thinking about how we, as humans, would solve the problem of grouping together stories about the same event.

I would venture to say that most humans, glancing at two stories, could look at the headline and know if they are about the same thing almost all of the time. In cases where the headline is uninformative, you’d scan the stories to see who or what is being talked about, and if it’s the same people or companies in both stories you’d probably read a little further to see if the same things are mentioned repeatedly in both stories. With this in mind, let’s think about how to build a similarity function which takes two documents as input, and returns a number indicating how similar the events being discussed in the articles are ...

##Designing a document similarity function

Let’s start with a simple (and not very good) similarity function. For each document, we could build a [sparse vector](http://en.wikipedia.org/wiki/Sparse_vector), called a [feature vector](http://en.wikipedia.org/wiki/Feature_vector), where each element in the vector corresponds to a particular word (‘jobs’) and its value is the number of times it occurred in the story (3). You could compare two documents by taking the [dot product](http://en.wikipedia.org/wiki/Dot_product) of their normalized feature vectors. This will result in a higher score for two documents which have more features (in our case, document words) in common. This particular similarity function would do a better job of telling you if the documents are topically similar than whether or not they are about the same event, but the basic framework is the right one.

So then what features should we be using to represent a document? The only way to know is to look at data. If you’ve already got a Prismatic account, you can go check out the global feed and you will see lots of examples of related stories clusters. If you’re not already a user ([what are you waiting for?](http://www.getprismatic.com/signup)), here are a few sets of related stories.

When you look at them, the question you should be asking yourself is: what features could I automatically extract such that the same features would be more likely to occur in two stories if those two stories were about the same thing?

#### Beer makes you smart

* Study says beer makes men smarter
* Make mine a double: How an alcoholic drink or two can sharpen up your mind
* DRINKING ALCOHOL MAY ENHANCE A PERSON’S PROBLEM SOLVING SKILLS
* Drink Wine to Increase Your IQ
* Study: Modest alcohol intake makes people more creative
* NO BRAINER: DRINKING BEER MAKES YOU SMARTER

#### Secret Service prostitution scandal

* 11 Secret Service agents put on leave amid prostitution inquiry
* Secret Service agents busted because they refused to pay hooker: source
* Secret Service agents relieved in Colombia amid prostitution allegations
* Secret Service:11 workers on administrative leave
* Secret Service Agents Recalled Because They Allegedly Didn't Pay Hooker
* Allegations of Prostitution Misconduct in Colombia: ‘Biggest Scandal in Secret Service History’

####Robert Doisneau's Google Doodle

* Robert Doisneau Google Doodle Pays Homage To French Street Photographer
* Robert Doisneau Google doodle marks centenary of his birth
* Monsieur! French photographer Robert Doisneau has centenary of his birth celebrated with Google Doodle collage of his pictures
* Robert Doisneau, an artist of the Parisian streets, honored today by Google
* Robert Doisneau Photos Featured in Google Doodle
* Google Doodle Pays Homage to Street Photographer Robert Doisneau

Here are some things you may have noticed:

* Images tend to be repeated in related stories.
* The same people and companies will occur in both articles. In other words, they share a similar set of entities (these are people, companies, etc). However, the set won’t be exactly the same, so seeing the same entities in the titles should be more indicative than seeing those entities in the text.
* Oftentimes for a given event there will be some phrase or quote that will get repeated a lot. A few months ago a Porsche got stuck in wet cement on the Embarcadero here in San Francisco, and every article about it had the phrases “wet cement” and “Porsche driver”. Both of these phrases are pretty rare in documents, and I don’t think any other documents in our collection at the time had the phrase “wet cement.” We have to be careful though, because there are many two word phrases that occur in all sorts of different articles, like “federal government” or “big data”. And when it comes to matching phrases - the longer the better. Many articles have direct quotes in them, and if two articles have the same direct quote, you know you have a good match.
A similarity function for related articles

Here’s a few more things to keep in mind: our title extractor is not perfect, and neither is our named entity extractor (which pulls out the names of people, companies, and locations). And we’re going to want to remove [stopwords](https://get-prismatic.squarespace.com/blog/2012/4/17/link), because having non-content words like ‘the’ in common is not very informative; and we might as well stem all the words while we’re at it, which means using the base form of the word and ignoring things like tense and case. Given all of this, let’s be precise about what a web page looks like after we have processed it for input to our system: we have title text and body text, where each has been simplified by having stopwords removed, along with tense/number. This text has been tokenized into a list of words. We also have possibly-wrong phrases from the title and the body corresponding to the people, locations and companies mentioned (raw tokenized text, not mapped to Wikipedia entries or anything fancy like that). Given all of the above, we have a pretty good idea of how to build a high-quality feature vector for determining document relatedness:

* features for each named entity unigram and bigram (this is just fancy talk for one word and two word phrases. n-gram means a phrase containing n words.)
* features for each title and text n-gram, where 3 <= n <= 8 (these numbers are somewhat arbitrary, but you get the idea: long enough phrases to be meaningful, but you gotta cut it off somewhere, so why not at 8?)

For each of these features, the value will be weighted according to it’s [tf-idf](http://en.wikipedia.org/wiki/Tf*idf) score, which is learned using a large corpus of documents, such as the ones that we process every day. The main purpose of using this score is that phrases which occur rarely will get weighted much more highly than phrases which occur frequently. Having “Pee-Wee Herman” in common with another document will result in a higher similarity score than having the phrase “Mitt Romney” in common. If you build a similarity function using this feature vector, it will do a very good job telling you if two documents are about the same event. Unfortunately, our problem is not yet fully solved. This similarity function is not very computationally efficient. The running time of this approach is dominated by feature computation, which in turn involves heavy string manipulation including regular expressions, which can be expensive. Keeping tf-idf scores for all observed three to eight word phrases means saving a whole lot of scores, more than you can keep in memory. And then to top it off, you have to compare each new document that comes into the system to every other document? That’s a lot of comparisons. So we’ve got to pare down the features and speed up the computation.

### Reducing Memory Consumption

Have you heard of [Zipf’s law](http://en.wikipedia.org/wiki/Zipf's_law)? Wikipedia does a good job describing it: “Zipf's law states that given some corpus of natural language utterances, the frequency of any word is inversely proportional to its rank in the frequency table.” What this means for us is that the bulk of the phrases we see will be a few distinct common phrases (and hence uninformative). Imagine you were to make a list of all observed phrases, along with the count of how many times you saw that phrase. If you removed the phrases that occurred only once, your list would be halved. If you then removed all the phrases which occurred only twice you would be halving your list again. So by removing rare phrases you can significantly cut down on your storage needs pretty quickly, but all rare phrases look the same regardless of whether you saw them zero, one or two times. This leads to a clever hack. We keep tf-idf counts for phrases seen more than some small number of times (where that number is largely determined by available memory), and for all other phrases we just treat them like previously unobserved (and hence potentially informative) phrases. Sure, we’re treating a phrase seen once or twice the same as one which is completely novel, and that’s suboptimal, but it’s less suboptimal that not being able to do the computation at all, and in practice it actually does nearly as well as the correct thing.

###Speeding up the computation

Now that we’ve solved our memory problem, we still need to solve our speed problem. Even if we don’t store every single n-gram, we still have to compute all the n-grams for a web page, and compare them to the n-grams for all the other web pages we have indexed, and that’s expensive. And a lot of the time, it’s also silly; we should be able to cheaply filter a lot of those web pages out of the consideration pool entirely. Let’s get back to that initial intuition - you could just read the title and quickly scan the article to see who’s mentioned before deciding between (a) definitely not a match (b) definitely a match, and (c) more investigation needed. The final model first computes similarity using only the title and named entity features, a pretty fast comparison, and then only if that similarity is above some threshold does it compute the similarity over the full feature set. This method filters about 95% of web pages from consideration, and makes the computation do-able in real time as the documents come in.

###Putting it all together

Alright, now let’s put it all together. At some point in the past, we took a bunch of documents and extracted all of these features -- named entity, title and text phrases of varying lengths -- counted how many times we saw them, and then saved the tf-idf information for the non-rare ones. These give us a sense of how rare/informative a phrase is. Then, each time a document enters the system, all of the features are computed for it, both the fast ones (NER and title features) and the slow ones (full feature set). We then compare it to every other document (we actually do a few other hacks to cut down on the set of documents, but I’m not going to go into that here) using just the fast features, and if that is above threshold then we compare them using the full set. If the similarity score on the full feature set is above some threshold, then we say that those two documents are about the same event. We have a bit more hackery to deal with merging and splitting clusters, which I’m not going to get into, but hopefully you could figure it out yourself by carefully examining situations where the model ends up with incorrectly merged/unmerged documents, looking for systematic clues which the model could use, and then figuring out how to efficiently incorporate those clues into the model.

### The results

Overall, we're very proud of our related stories clustering system. It's not perfect, but that's always the case when dealing with natural language. It's very good though, and I am constantly impressed by some of the document clusters it correctly identifies. 

Working on natural language processing at Prismatic has been loads of fun. We're solving a lot of very challenging problems, all aimed at finding you the very best content possible. Thanks for taking this wild ride with us.
