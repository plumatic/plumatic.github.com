---
layout: post
title: 'Prismatic Partner Series: How SimpleRelevance Uses the Prismatic Analysis
  API'
date: 2015-07-10 18:41:02.000000000 -07:00
---
We’re always on the lookout for interesting applications of the Interest Graph API. We’re putting together a growing list of guest posts from our partners. [Let us know](mailto:public-api+partners@getprismatic.com) if you have an interesting or unique story that you would like to share with the community.

Today’s guest post is written by Erich Ess, Chief Technology Officer and Melanie Teachout, Marketing Manager of SimpleRelevance. [SimpleRelevance](https://www.simplerelevance.com/) personalizes marketing messages for retail, financial, and media companies. They take clients’ data about customers, and for each customer predict the content that will each customer will most likely engage with. Prismatic’s Interest Graph Analysis API plays an essential role in their machine learning engine.

## Delivering Value to Our Clients with Prismatic’s Accurate, Consistent, and Easy-to-use Analysis API

One specific Fortune 500 media company enlisted our machine learning engine to personalize their news article suggestions within emails. They wanted to specifically increase click through rates and interactions on their website. To do this, we collected data about their readers, and all the articles they were publishing, then fed that data into our machine learning engine to predict which newly published articles should be sent to each reader. After we began personalizing emails, we saw increased reader interactions with recommended articles. When trying to find ways to further improve the client’s performance, we noticed the descriptive data was inconsistently tagged. A common challenge with tagging is that it’s not always consistent. For example, an article about the President could be tagged "Obama," “President,” “President Obama,” or “POTUS.” This makes finding correlations and similarities between different content items more difficult, which in turn makes it more difficult for SimpleRelevance to recommend appropriate content for readers. We saw this as a valuable opportunity for improvement that could significantly boost the effectiveness of our engine.  


###Using Prismatic’s Interest Graph API to Analyze and Extract Descriptive Tags 

SimpleRelevance had three options to optimize tagging. We could build our own natural language processing (NLP) engine to analyze text content and extract topics, but that would take substantial effort. Our second option was creating a tag clean up process for each client by setting up rules to merge the equivalent tags into one single tag, but that would have been extremely manual and wouldn’t have helped for clients who do not tag their content. Our third option was to use Prismatic’s Interest Graph API to analyze and extract descriptive tags for each item of content they collect from clients. We ultimately chose to save time (because time really is money) and work with Prismatic to use those tags as descriptive data within our machine learning models.

After beginning to use Prismatic’s platform, we found it is easy and convenient because it allows us to pass the full article of text or a URL through a simple API and then returns a list of topics. We verified Prismatic’s efficacy and accuracy by manually tagging a small sample of articles and comparing our tags to those returned by Prismatic’s system. Prismatic uses a global pool of topics for analyzing all articles, which ensures tagging consistency across all of their clients. Through using Prismatic’s Interest Graph API, we’ve been able to significantly improve the consistency of tags for all content of our clients and easily and efficiently improve the performance of our personalization platform. Not only have we seen great overall value from Prismatic in our own technology, but our own clients are reaping the benefits as well.

