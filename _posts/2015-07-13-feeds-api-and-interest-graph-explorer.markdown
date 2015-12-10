---
layout: post
title: Introducing the Feeds API and Interest Graph Explorer
date: 2015-07-13 08:49:14.000000000 -07:00
---
TLDR: Today we have two new releases: (1) an addition to our Interest Graph API to serve a feed of content about any given topics or aspects and (2) the [Interest Graph Explorer](http://prismatic.github.io/explorer/), a demo application built using the Interest Graph API to help visualize these feeds and the accompanying data. Sign up for an API token to start getting high-quality, fully-analyzed content today!

#Introducing the Feeds API and Interest Graph Explorer

Over the past several months, we've been releasing components of our Interest Graph API and powerful content analysis tools. Our analysis APIs can take raw text or a URL and determine the [topics](http://blog.getprismatic.com/interest-graph-api/) (e.g. *gadgets*, *travel*, or *programming*) and structural [aspects](http://blog.getprismatic.com/deeper-content-analysis-with-aspects/) (e.g. *recipe* or *news article*) of the content. Today we are excited to offer access to another component to our Interest Graph, the Prismatic Feeds API.

Looking for a feed of stories about [*watches*](http://prismatic.github.io/explorer/?topics=4789), or a collection of [*recipes* about *chocolate*](http://prismatic.github.io/explorer/?topics=915&aspect=type.article.content.recipe)? The feeds API provides extracted metadata for popular web content -- including title, text, and images -- in addition to the topics and aspects from our analysis API. With the new feeds API, you can create customized feeds of the latest, high-quality content that match a combination of topics and/or aspects that you specify using a simple, yet powerful query language. The content in feeds is highly customizable; it can be narrowed down to match a few related topics and a particular aspect, or the content can be more general and cover a wide range of interests and page types.

The feeds API opens access to our enormous index of documents that have already been analyzed and indexed with topics and aspects from our analysis API. It is the same technology that has been at the heart of Prismatic’s [iOS](https://itunes.apple.com/us/app/prismatic-personalized-social/id551206444), [Android](https://play.google.com/store/apps/details?id=com.Prismatic.android), and [web](http://getprismatic.com/) consumer apps for years.

These documents have recently been shared on social networks or RSS feeds and pass a certain quality bar derived from social and content signals. We consider millions of pages every day and add hundreds of thousands URLs per day to be served in the feeds.

To help visualize the feed data and showcase some of the capabilities of the Interest Graph, we built the [Interest Graph Explorer](http://prismatic.github.io/explorer/). It's a simple application that can be used to create a custom feed of articles that match the union of topics from the search bar and the selected aspect. For example, you can create a general [feed about watches](http://prismatic.github.io/explorer/?topics=4789), or you can get a more specific feed of [product reviews about watches](http://prismatic.github.io/explorer/?topics=4789&aspect=type.article.content.review) by using the *review* aspect. If reviews about watches aren't your thing, we have a handful of other aspects and thousands of other topics to explore.

## Applications

We are impressed by the impact of applications built on our analysis APIs. For example, one of our users is using topic analysis learn a per-user model in order to build personalized marketing campaigns for their enterprise clients. The analysis API already supports many high-value use cases, and our new feeds API will enable a whole new set of applications.

For example, we've been working with a financial technology startup that offers a dashboard application that provides valuable data to hedge fund and portfolio managers. Nearly every competitive financial manager already receives traditional analyst research reports like those provided by Goldman Sachs and JPMorgan. However, the most innovative financial companies are seeking to get supplemental sources of information providing them a competitive advantage. The feeds API addresses this need by automatically aggregating timely, high-quality news from an extensive collection of sources that is organized into thousands of topics, so these managers can get up-to-the-minute information on new developments that are not covered by the traditional research houses.

Another application pertains to an entrepreneur running an online community where gardeners can connect and learn from each other. Her product helps keep users engaged by providing a feed of relevant content for users to share and discuss. By showing a feed of stories that are of genuine interest to her users and aligned with her gardening community brand, she organically strengthens the associations her users have with her brand. The content is relevant, it has been vetted through social network quality measures, and is likely to be shared again. This fresh content grabs and holds the attention of her target users and keeps them coming back for more. 

These are just a few example applications that the feeds API supports. These feeds of fresh stories are an amazing data source that can power tons of other use cases. The possibilities are endless.

## Getting Started

You can explore the new feeds API immediately with the [Interest Graph Explorer](http://prismatic.github.io/explorer/), or query it programmatically by following the instructions below.

### Programmatic Feeds API Access

#### Get a Token 

If you haven’t already, [sign up for an Interest Graph API token](http://interest-graph.getprismatic.com/).

#### Programmatically Get Stories from a Feed 

Feeds are requested with a simple query language. Here’s an example request for all content about *machine learning* (topic):

[Preview in Interest Graph Explorer](http://prismatic.github.io/explorer/?topics=2620&page=1)

```
POST /doc/search
{
  "query": {"topic": 2620}
}
```

Let's narrow the content to only *news articles* (aspect) about *machine learning* (topic):

[Preview in Interest Graph Explorer](http://prismatic.github.io/explorer/?topics=2620&aspect=type.article.content.news&page=1)

```
POST /doc/search
{
  "query": {
    "and": [
      {"topic": 2620},
      {"aspect": "type.article.content.news"}
    ]
  }
}
```

It's easy to add more topics to this feed. Let's also include news articles that also match *functional programming* and *artificial intelligence* topics:

[Preview in Interest Graph Explorer](http://prismatic.github.io/explorer/?topics=2620,308,1656&aspect=type.article.content.news&page=1)

```
POST /doc/search
{
  "query": {
    "and": [
      {"aspect": "type.article.content.news"},
      {
        "or": [
          {"topic": 2620},
          {"topic": 308},
          {"topic": 1656}
        ]
      }
    ]
  }
}
```

For this release, we intentionally scoped the query language to just topics and aspects while exposing an interface that allows us to add new capabilities down the road.

[Click here](https://github.com/prismatic/interest-graph#do-you-have-the-topic-i-care-about) to get a sense of the full set of topics and aspects we offer.

## What’s Next?

The analysis API focuses on extracting structure from a single document; the feeds API empowers you to generate recommendations from an index of popular content. Ultimately, the most value comes from building custom collections of your content. The architecture supporting the feeds API easily scales, and the indexing system will allow us to support custom, user-generated collections in the future.

Initially, uploading a custom collection to Prismatic will enable easy organization and standardization your collection. But it also will enable generating personalized recommendations from your collection that can be tailored to each of your users' interests. 

We’re already talking to a couple of large publishers about recommending content from their own collections of documents. Let us know if you would like to see this functionality included in our future releases.

## Send Feedback 

Our goal is to make the Interest Graph as useful as possible, and that works best with your input. We would love to learn about your use cases and get feedback on additional features that you would like to see. Get in touch at [public-api@getprismatic.com](mailto:public-api@getprismatic.com) or on Twitter [@PrismaticEng](https://twitter.com/prismaticeng).

