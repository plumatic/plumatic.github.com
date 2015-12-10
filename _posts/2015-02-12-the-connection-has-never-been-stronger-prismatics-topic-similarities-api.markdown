---
layout: post
title: 'The Connection Has Never Been Stronger: Prismatic''s Topic Similarities API'
date: 2015-02-12 11:21:23.000000000 -08:00
---
Since [announcing the Interest Graph](http://blog.getprismatic.com/interest-graph-api/) last week, we’ve been thrilled with the great number of individuals and companies who have [signed up](http://interest-graph.getprismatic.com/) and already started tagging their URLs and text with topics.  

Today, we’re excited to release the first of many more edges we’ll be adding to the Interest Graph: related topics.
<center>
<img src="/content/images/2015/02/image_0.png" style="width:30%;">
</center>

With this new endpoint, you can query for topics related to a given query topic. For example, querying for *Cats* (id 809)

```

curl -H "X-API-TOKEN: <API-TOKEN>" 'http://interest-graph.getprismatic.com/topic/topic?id=809'

```

yields related topics including *Kittens, Dogs,* and *Pets*.

### Applications

Here are some example applications of related topics:

#### Recommending Topics

Similar topics can be used to automatically increase the pool of topics that are returned from a query. For example, if you run the topic tagger over a document and want to expand the set of returned topics, you can compute the topic similarities for each returned topic and aggregate the results to get a broader set of topics. Alternatively, if a user searches for topics, related topics can bolster the search results so the user is more likely to find the relevant topic. At Prismatic, we build user profiles by tracking the topics favorited by a user, and we recommended additional topics for users to follow by computing the topics similar to the user’s favorites.

	

#### Recommending Content

The similarity between topics can be lifted to compute a similarity between other entities. For example, if two articles are tagged with different sets of topics, a simple measure of topic-overlap might suggest that the articles are completely different. Measuring similarity by first expanding the set of topics on an article can detect similar articles that would otherwise be missed. In the example below, we can detect that article A is similar to article B because they both are indirectly talking about *Pets*.

![image alt text](/content/images/2015/02/image_1.png)

If you have other content labelled with topics -- such as advertisements -- you can use the same similarity expansion to find a broader set of articles for which the advertisement is relevant. Beyond simply matching an article or ad with a given article, you can also match users with other users who have similar sets of favorited interests.

	

#### Diversification

Whenever you are automatically recommending content to a user, you’re making a bet that they find the content interesting. As with any data-driven approach, this bet can sometimes fail. A popular technique for hedging the bet is to present a diverse set of recommendations to the user. Topic similarities can be used to remove similar content in order to diversify the set of recommendations presented to the user. 

[Check out our FAQ](https://github.com/Prismatic/interest-graph#search-for-topics-related-to-a-given-topic) for the technical details of how you can begin recommended related topics based on a query topic. [Sign up](http://interest-graph.getprismatic.com/) to start using the topic similarities today.

We are planning to release a bunch more functionality in the coming weeks and months, so stay tuned. [Let us know](mailto:public-api@getprismatic.com) what you think, and what functionality you would like to see.

