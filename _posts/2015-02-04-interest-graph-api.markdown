---
layout: post
title: Announcing the Interest Graph API
date: 2015-02-04 09:56:59.000000000 -08:00
---
tl;dr: 
Today we’re taking the first step in opening up our interest graph by releasing an API that automatically identifies the thematic content of a piece of text.  [Sign up](http://interest-graph.getprismatic.com/) for an API token to start tagging your text.

###Our Expertise in Your Hands

At Prismatic we’ve spent a long time thinking about how to provide our users with the most relevant recommendations. To do this, we’ve engineered the interest graph -- a model that helps us understand our users, their interests, publishers, content, and the connections between them. By aligning with users’ interests, the interest graph enables recommendations of products and content to deliver an experience that people care about. Today, we’re releasing the first building block of our interest graph, the connection between content and interests.
When we built the interest graph for Prismatic, we wanted to find interests that people identify with. Most existing taxonomies were either too specific (e.g., Wikipedia) or too task-focused (e.g., ads targeting), so we decided to build our own. We surveyed the popular newspaper categories that have been used to classify articles for centuries, supplemented this list with the top liked pages on Facebook, and added the most popular search queries from the Prismatic app. The result is the most comprehensive list of interests that people care about on the web today.
These interests are single-phrase summaries of the thematic content of a piece of text; examples include Functional Programming, Celebrity Gossip, or Flowers. Interests provide a short, meaningful summary of the content of an article, so you can quickly get a sense for what it’s about without spending the time reading it. By providing a short, intelligible summary, interests lend useful structure to otherwise raw, unstructured text.
We have received many requests to open our interest graph to external developers. Today, we’re happy to announce an ALPHA version of the same [interest graph](http://interest-graph.getprismatic.com ) powering Prismatic. We are excited to see the creative and new projects that will come from getting our interest graph into the hands of developers.

###Applications

By automatically tagging your collection of articles with interests, you introduce additional structure that can be used in a variety of ways.

**Organizing Content**

Readers of your articles can more easily find the content that they care about by narrowing in on articles tagged with a particular interest. A lot of blogs already tag their posts with interests so readers can quickly know what an article is about, and to quickly find other articles from the same interest. For example, take a look at the the Photography and Surreal tags at the top of the following post:

![]({{site.baseurl}}/content/images/2015/02/image00.png)

Traditionally, these articles are manually tagged with interests resulting in a non-canonical, open class of tags, which makes searching for them hard. Having the interest graph automatically tag posts helps maintain a standard that keeps things organized and lends itself well to searching.

Most articles lie at the intersection of multiple interests; for instance, the article above is about both Photography and Vintage. By navigating to these related interests, a reader can stumble upon new interests and discover interesting articles in a unique and serendipitous way.

**Find Related Content**

You can encourage your readers to explore by showing related articles (e.g. in the sidebar); interest tags make automatically selecting related articles easy. There are various similarity metrics that you can use to find related docs to a given doc. For example, treating the interests on two separate articles as sets A and B, you can use the [Jaccard Index](http://en.wikipedia.org/wiki/Jaccard_index) to measure the similarity between the two articles:

![]({{site.baseurl}}/content/images/2015/02/Screen-Shot-2015-02-04-at-10-44-20-AM-1.png)

Computing similarity of articles by comparing their interest tags is less susceptible to noise and can be significantly more efficient than computing similarity based on the words in the articles.

More generally, the set of interest tags serve as stable categories for finding related content to pair with an article. For instance, by labeling advertisements with the interest tags to which they are relevant, you can automatically match them with the articles tagged with those interests. Similar techniques can be used for products or other related content to layout beside a given article. 

**User Profiles**

Interests provide a means to deeply model user preferences. At Prismatic, we model our users’ interests by analyzing how they interact with documents. To get a sense of how article interactions reflect a user’s interests, take a look at some of the top interests we automatically extracted from the links shared on the following celebrities’ twitter accounts:

![]({{site.baseurl}}/content/images/2015/02/Screen-Shot-2015-02-04-at-10-06-40-AM.png)

**Much More...**

Overall, there are tons of applications of interest tags. Whether you want to organize your collection of articles, help users retrieve or discover the most relevant information, quantitatively analyze a collection of unstructured text data, or get a sense of your users’ preferences, interests are a great way to approach the problem. These are just a few ideas about how interest tags can be used, but there are countless more. How will you use interests?

###Getting Started

In order to use the interest graph, you must first get an access token. Then you can submit queries either programmatically or via a web interface for URLs or a chunk of text to tag. The response has a list of the interests detected in the text with additional metadata.

Step 1: Acquire an access token

Head over to http://interest-graph.getprismatic.com, enter your email address, and some additional info about how you plan to use the interest graph, and we will email you an API access token.

Step 2: Make a query

Once you have your access token, you can try tagging a URL or piece of text via our web interface. The same http://interest-graph.getprismatic.com has input fields where you can enter the query URL or text. 

You can also make requests programmatically. For example, if we want to run the interest graph on the Wikipedia article about Machine Learning: http://en.wikipedia.org/wiki/Machine_learning, we can curl the service:

`curl -H "X-API-TOKEN: <API-TOKEN>" 'http://interest-graph.getprismatic.com/url/topic' --data 'url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMachine_learning'`

where the `<API-TOKEN>` is a stand-in for the access token string.

Step 3: Interpret the response

The response comes in the form of a JSON map, with a key `topics` that has a list of topic tags. Each topic tag has a numeric `id` of the topic in the system, a human-readable topic name `topic`, and a `score`. The score is a real value between 0 and 1, and represents the degree to which a significant part of article is about the corresponding topic.

Here is an example response:


<script src="https://gist.github.com/davegolland/436cfd899078bf8465e8.js"></script>


**Under the Hood**

Tagging an article with interests is not a trivial task. We can’t just look at whether or not the interest name is present in the article. Firstly, this approach would result in many false positives. We don’t want to say an article is about something if it just mentions that interest in passing. What’s more, words have different senses -- for example, the noun “bar” can refer to that crowded pub on the corner or a unit of some product (e.g. chocolate, soap). In order to determine the interests of an article, we need to look at the collection of words in the article as a whole. Secondly, tagging based exclusively on the presence of the interest name can also result in false negatives -- an article may be about an interest, even if it does not explicitly mention it by name. For example, there are articles discussing how greenhouse gases play a role in changing the climate, which are ultimately about Global Warming even though these words are never explicitly mentioned. We need more sophistication from our model to avoid these false positives and negatives.

Instead, each interest is implemented as a learned, statistical model that can automatically analyze the content of an article holistically. The models are trained by analyzing many real documents from across the web to see how people actually talk about certain interests in practice. The training of the interest models has been fully automated; when we want to construct a new interest model, we simply specify an interest name. Our training pipeline automatically collects and selects positive and negative training examples from around the web and trains a classifier that can be used to classify whether or not a new article is about the interest.

We currently have over 5k high-quality, modeled interests that are retrained on a regular basis to ensure they are up to date. Of course, the set of interests discussed in the world is constantly changing, and over time we plan to gradually grow the set of modeled interests accordingly. If there are some interests that you would like to see added, please visit our [FAQ](https://github.com/Prismatic/interest-graph/) to see how you can request their addition.

**Caveats and Limitations**

While in most cases we provide automatic tagging of interests from a URL, we respect individual publisher preferences to avoid being crawled by automated systems -- and therefore we may fail to extract text from a URL. In such situations, you can extract the text from the page yourself, and run the extracted text through our text-query interface.

Our approach to interest modeling is inherently data-driven and, as with all data-driven models, it is subject to some noise. Thus, there are some articles that might be mis-tagged with incorrect interests, and some articles whose content reflects a particular interest that our models fail to detect. We believe that on the whole, these models do a good job, but errors are inevitable. We will record all reported errors in order to feed them back into our training pipeline to ensure it improves over time. If you would like to learn how to report an error, please visit our [FAQ](http://interest-graph.getprismatic.com/faq).

Currently, the set of interests is fixed. Given our resources, we don’t currently model every imaginable interest. In order to search through the set of interests our system currently models, please visit our [interest search page](http://interest-graph.getprismatic.com/topic/search/human).

**Looking for Feedback and Suggestions**

We are still in the early stages of opening up our infrastructure, and appreciate any and all feedback. We are particularly interested in hearing about any broken functionality or errors in tagging. We are also curious to know which interests you would like added, and how you plan to use the automatic interest tagging system. Let us know what you think at public-api@getprismatic.com.
