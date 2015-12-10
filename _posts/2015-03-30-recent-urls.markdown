---
layout: post
title: Interest Graph Topic Feeds API
date: 2015-03-30 16:11:10.000000000 -07:00
---
TLDR: Today we are introducing the recent URLs endpoint to the Interest Graph, which empowers *you* to find and recommend the best recent content about topics of interest.  [Sign up for an API token](http://interest-graph.getprismatic.com/) to start getting the freshest content about your interests.

## Announcing Recent URLs

Before today, the Interest Graph has focused on analyzing input content. From a URL or text input, the Interest Graph categorizes the content into topics or aspects, which can lead to useful business insights. Although these analyses are useful, they are *derived* -- they require either a URL or text as input to be useful. On the other hand, content is primary and can stand alone. Insights are good, but content is king.

 <table style="width:400px;">
    <tr>
          <th>Interest Graph</th>
    </tr>
    <tr>
          <td><img src="{{site.baseurl}}/content/images/2015/03/image_0-1.png"></td>
    </tr>
</table>

Today, we’re releasing the recent URLs endpoint, which provides recent, high-quality URLs from all over the web for a given input topic. The recent URLs endpoint takes as input a [topic](https://github.com/Prismatic/interest-graph#do-you-have-the-topic-i-care-about) from our Interest Graph, and returns a list of URLs about that topic that have recently been shared on social networks and pass a certain quality bar derived from social and content signals.

The technology powering the recent URLs endpoint is the same technology that has been at the heart of our [iOS](https://itunes.apple.com/us/app/prismatic-personalized-social/id551206444), [Android](https://play.google.com/store/apps/details?id=com.Prismatic.android), and [web](http://getprismatic.com) consumer apps for years. For example, here is the *[Machine Learning topic feed](http://getprismatic.com/topic/machine+learning)* showing recent stories from around the web tagged with the *Machine Learning* topic. With this release, you can conveniently query our content ingestion and aggregation pipeline and use the returned URLs in a variety of applications.

## Applications:

### Consumer Apps

Ever since the digital revolution has democratized publishing, the quantity and diversity of content produced has grown rapidly. As the sources of content become more dispersed, people have to check an increasingly large number of sources to stay up-to-date on all the latest happenings. To save consumer time and effort, quite a few news aggregators have emerged that pull together content from the long tail of sources into a unified "reader-app" that pushes content to the consumer.

At Prismatic, we have built such an app. We placed an enormous amount of engineering investment into the document ingestion and analysis pipeline. We crawl a continuously growing pool of sources, fetch the freshest, high-quality articles, tag them with topics, and index them for fast retrieval. We chose to build a smart reader app on top of this aggregation pipeline that learns from user activity to continuously improve the feed ranking. Our investment has paid off; the Prismatic app is an engaging product that has served over a billion pieces of content to our users.

Our particular smart-reader implementation is just one approach among many for building an engaging newsreader. The overhead of building a doc-ingestion and aggregation pipeline has hindered others with creative ideas. The recent URLs endpoint lowers this barrier to entry, and enables more recommendation products and features. Whether their strength is visual design, user interfaces, or smart feed ranking, entrepreneurs can focus on their core competencies by building on top of the Interest Graph’s recent URLs endpoint.

### Content Marketing

Marketers have learned that traditional advertising techniques are losing their effectiveness; people are simply becoming desensitized to ads. Today’s marketers now drive sales by relying more heavily on content marketing. They publish relevant content that is aligned with their brand and of genuine interest to their customers. This content grabs and holds the attention of their target audience, and keeps them coming back for more. 

Unfortunately, producing original, high-quality content on a daily basis is extremely costly, and finding consistently high-quality inspiration is difficult. There is no mythical "source of all inspiration," instead it comes from other content produced by people all over the world. Fortunately, the recent URLs endpoint aggregates the newest content and organizes it into all the topics in the Interest Graph. The content is relevant, it has been vetted through social network quality measures, and is likely to be shared again.

Sharing content on social media not only increases a brand’s web-presence, but it also provides a means for building rich and unique brand associations. Marketers can educate their audience with content that aligns with their company’s message. Empowered with this knowledge, customers may greater appreciate the value of a company’s product, and ultimately this knowledge will influence their purchasing decisions.

### Information Advantage
	
There are many businesses that derive value primarily from new information. These businesses transform information from knowledge of new events into a competitive advantage. Most traditional news sources report only the biggest news events, which exclude the vast majority of smaller, more niche developments. Hence, businesses seeking information advantage must cultivate alternative news sources that provide signals about these smaller developments. The recent URLs endpoint addresses this need by automatically aggregating timely, high-quality news from an enormous collection of sources organized into thousands of topics that align with niche interests.

For example, managers of investment funds analyze markets to determine which investments to buy and sell. Information about current events impacts the share price of these investments. As an extreme example, the [death of a CEO can cause enormous swings](http://deepblue.lib.umich.edu/bitstream/handle/2027.42/25717/0000274.pdf) in share prices. Of course less dramatic events -- such as changes in a brand’s public perception or developments in related industustries -- can impact share price as well. Becoming aware of a news event early provides an enormous competitive advantage for trading before others learn about the information, and the market reaches equilibrium.

## Getting Started

### Get a Token

If you haven’t already, [sign up for an Interest Graph API token](http://interest-graph.getprismatic.com/). 

### Programmatically Get Recent URLs for a Topic

To programmatically get recent URLs, issue a GET request to http://interest-graph.getprismatic.com/topic/recent-url as demonstrated in the following curl request, which gets the recent URLs for the *Machine Learning* topic (id 2620):

`curl -H "X-API-TOKEN: <API-TOKEN>" 'http://interest-graph.getprismatic.com/topic/recent-url?id=2620'`

For more information about which topics we offer, please visit the [FAQ](https://github.com/Prismatic/interest-graph#do-you-have-the-topic-i-care-about).

## Analyze the Response 

Here is an example JSON response from the recent URLs endpoint:

<table style="width:400px;">
  <tr>
    <td>JSON Response for URLs tagged with Machine Learning</td>
  </tr>
  <tr>
    <td><img src="{{site.baseurl}}/content/images/2015/03/Screen-Shot-2015-03-30-at-2-42-31-PM.png"></td>
  </tr>
</table>


The response is a map with a `urls` key that maps to a list of scored URLs. The score is a real value between 0 and 1, and represents the degree to which a significant part of article is about the corresponding topic. 

In this case, we get URLs pertaining to *Machine Learning* such as those discussing how quantum computers could accelerate machine learning, a Huffington Post article exploring what you can do with data, and an article about Andrew Ng presenting a recipe for successfully applying machine learning.

## Send Feedback 

Our goal is to make the Interest Graph as useful as possible, and that works best with your input. We would love to learn about your use cases and get feedback on additional features that you would like to see. Think you know anyone whose business might drive incremental revenue through better models powered by our APIs? We’d love to learn about the use case and brainstorm application ideas. Get in touch at [public-api@getprismatic.com](mailto:public-api@getprismatic.com).

