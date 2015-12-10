---
layout: post
title: 'Deeper Content Analysis with Aspects: Interest Graph grows beyond Topics'
date: 2015-03-09 13:56:50.000000000 -07:00
---
TLDR: 

Today we’re taking the next step in opening up our Interest Graph by releasing an aspect tagging API that automatically identifies the structural content of a piece of text. [Sign up for an API token](http://interest-graph.getprismatic.com/) to start classifying your URLs.

## Announcing Aspects

Last month, we started [open](http://blog.getprismatic.com/interest-graph-api/)[ing](http://blog.getprismatic.com/interest-graph-api/)[ up the ](http://blog.getprismatic.com/interest-graph-api/)[Interest Graph](http://blog.getprismatic.com/interest-graph-api/) -- a model that helps us understand our users, their interests, publishers, content, and the connections between them. Our first two releases have focused on "topics," which model the semantic subject of a piece of content. We’ve been blown away by the response from the community: several hundred users have tried the Interest Graph API, and some have gone on to analyze millions of pieces of content in areas as diverse as music, recipes, and news. 

These areas are just a few examples demonstrating the structural variety in content out there on the web. In addition to tagging content according to what it’s about, content can be tagged according to its structure or function. Today we are happy to release another node in the Interest Graph that captures these variations: *aspects*.


 <table style="width:400px;">
    <tr>
          <th>Interest Graph</th>
    </tr>
    <tr>
          <td><img src="/content/images/2015/03/image_0.png"></td>
    </tr>
</table>

The topics and aspects represent deep analysis of the content of a webpage into a set of domain-specific concepts that can drive revenue. Examples of these revenue-driving business use cases include models for cart upsell or related products in ecommerce, extremely relevant content recommendations, setting the optimal floor price for an ad auction in media, and linking the securities and corporate events mentioned in financial news to build new predictive models.

## Background: Topics vs. Aspects

Our existing API automatically tags text or URLs with *topics* that capture thematic content. For instance, our introductory [blog post](http://blog.getprismatic.com/interest-graph-api/%27) is about *Text Mining*, *Semantic Web*, and *Information Retrieval*. Topics capture the subject of the text of the blog post, but say nothing about the post’s form or function. Along these dimensions, the post can be described as an *Article* that announces a new software release. Aspects, such as *Article*, automatically capture the structural dimension along which recipes, interviews, videos, and product pages differ from one another. Aspects can analyze the complete structure of the DOM to extract rich and actionable information. 

As an example, the following two articles are about different topics, but share the same structural aspect; they are both product *Review*s.

![](/content/images/2015/03/Screen-Shot-2015-03-10-at-10-00-24-PM.png)

<table>

  <tr>
    <td>Topics: Cycling, Commuting</td>
    <td>Topics: Photography, Digital Cameras</td>
  </tr>
  <tr>
    <td>Aspect: Review</td>
    <td>Aspect: Review</td>
  </tr>
</table>

<br>

Automatically tagging articles as *Review*s is just of the many uses of today’s release. This initial release includes aspects for the most popular content types on the web today that can be employed in a host of applications. Let’s take a look at some applications and then dive deeper into the structure of the aspects that we’re releasing today.

## Applications

In the age of ecommerce and digital media, companies are struggling to keep up with the pace at which content is generated. Many businesses manage copious amounts of unstructured data on sites they own and operate as well as those in their extended network. Without the proper tools, it is hard for these companies to fully extract value from their data. 

By providing high-quality classifications of web pages in real-time, the Interest Graph helps unlock this value in a variety of applications such as the following. 

### Content Organization

Aspects help businesses gain visibility into the assortment of content under their purview. For example, the Interest Graph will automatically classify web pages that are selling a product such as a song, gadget, or book with the *Commerce* aspect. These pages are distinguished from those tagged with the *Video* or *Audio* aspects. The Interest Graph has aspects for *Review*, *Recipe*, and *News* articles as well. 

Content aggregations can use these aspects to organize and link content with other content types. For example, aggregators can link *Review Articles* with their target *Product* or even recommend other *Product*s related to the *Review*. Moreover, by looking at the intersection of aspects and topics, content can be organized around interesting themes such as *Review*s about *Gaming*, or *Products* related to *Running*. 

### Personalization

By studying the relationship between content and consumer, businesses can more accurately target users and increase their access to the most relevant content. For example, classifying content with aspects provides informative metadata that facilitates organizing, browsing, or searching the content. Moreover, when analyzing how different consumers engage with different types of content, patterns emerge about personalizing the relevance of content to different segments of the consumer base. These patterns can be used to deliver the most useful content to the correct users.

### Pricing

Many digital media businesses monetize their web properties by selling screen real estate to advertisers. Real estate is limited, and these businesses must choose between many potentially relevant ads. Accurately pricing the real estate is a challenging problem that often depends on the content of the article -- the more relevant an ad to the article, the more valuable it is. Deep analysis of the content of the article with aspects can help automatically predict an accurate price for the real estate.

### Feedback

After choosing content or ads to present to consumers, many businesses collect data to evaluate how well the choices performed. Properly analyzing the data provides feedback for future choices. Deep analysis of the content with aspects can uncover valuable insights previously undetectable in the data. For example, the analysis of the data can inform which experiments to run or which techniques work best for recommending content. By improving the quality of future recommendations, businesses can reduce wasted impressions and ultimately maximize their return on investment.

## Aspect Hierarchy

The Aspect Hierarchy organizes the web into a taxonomy of classes. It is structured from general to specific, where each class (e.g. *Article*) can be further refined into subclasses based on more specific attributes (e.g. *News* vs. *Interview*).  

Here is a diagram of the aspects we’re releasing today.

![image alt text](/content/images/2015/03/image_1.png)

Each oval represents a class of webpages, and each diamond is an attribute that
further partitions the webpages of its parent into mutually exclusive
subclasses. For example, every webpage has exactly one *type* (e.g. *Image*,
*Article*, *Commerce*, or *Other*), and every *Article* is further classified
into a single *content* type. Therefore, a webpage can’t be both an *Event* and
a *Review* because it can’t have *type* both *Commerce* an *Article*, but it
can be both an *Event* and *Risque*.

Currently, there are two top-level classifications: *type* and *flag_nsfw*:

The *type* attribute partitions webpages into mutually exclusive sets of
content types according to the primary focus of the webpage.

<table>
  <tr>
    <th>Content Type</th>
    <th>Primary Focus of Webpage</th>
    <th>Example URL</th>
  </tr>
  <tr>
    <td><i>Image</i></td>
    <td>image</td>
    <td><a href="http://the-toast.net/2015/03/02/emily-dickinson-dining-cartoon/">example</a></td>
  </tr>
  <tr>
    <td><i>Article</i></td>
    <td>textual content</td>
    <td><a href="http://www.bbc.com/news/technology-31620759">example</a></td>
  </tr>
  <tr>
    <td><i>Audio</i></td>
    <td>audiofile such as song, podcast</td>
    <td><a href="https://itunes.apple.com/us/album/hearts-i-leave-behind-feat./id969744516">example</a></td>
  </tr>
  <tr>
    <td><i>Video</i></td>
    <td>video</td>
    <td><a href="https://www.youtube.com/watch?v=rI8tNMsozo0">example</a></td>
  </tr>
  <tr>
    <td><i>Commerce</i></td>
    <td>offer a product or other entity </td>
    <td><a href="https://www.etsy.com/listing/223653209/complete-set-golden-girls-prayer-candles">example</a></td>
  </tr>
</table>


The *Article* class is further refined according to the primary focus of the content of the text.

<table>
  <tr>
    <th>Type of Content</th>
    <th>Primary Focus of Content</th>
    <th>Example URL</th>
  </tr>
  <tr>
    <td><i>Review</i></td>
    <td>review of a product, piece of media, or app</td>
    <td><a href="http://magazine.good.is/articles/leave-the-bees-in-peace">example</a></td>
  </tr>
  <tr>
    <td><i>News</i></td>
    <td>story about a recent or significant event</td>
    <td><a href="http://www.bbc.com/news/technology-31620759">example</a></td>
  </tr>
  <tr>
    <td><i>Recipe</i></td>
    <td>instructions for preparing a dish</td>
    <td><a href="http://www.uncommondesignsonline.com/chocolate-banana-cupcakes-peanut-butter-icing/">example</a></td>
  </tr>
  <tr>
    <td><i>Deal</i></td>
    <td>timely savings on product or service, but not a direct page where the product can be purchased</td>
    <td><a href="http://www.focusedonthemagic.com/2015/02/kingdom-camera-rentals-giveaway-fun.html">example</a></td>
  </tr>
  <tr>
    <td><i>Interview</i></td>
    <td>content presented in a question and answer format</td>
    <td><a href="http://whiskyspeller.blogspot.nl/2015/02/this-place-in-south-africa-where.html">example</a></td>
  </tr>
  <tr>
    <td><i>Listicle</i></td>
    <td>content presented in a numbered or bulleted list</td>
    <td><a href="http://www.buzzfeed.com/javiermoreno/i-love-sleeping">example</a></td>
  </tr>
</table>


Each webpage in the *Commerce* class is partitioned based on the product that is offered.

<table>
  <tr>
    <th>Entity Offered</th>
    <th>Description of Entity</th>
    <th>Example URL</th>
  </tr>
  <tr>
    <td><i>Product</i></td>
    <td>a tangible item for purchase</td>
    <td><a href="https://www.etsy.com/listing/223653209/complete-set-golden-girls-prayer-candles">example</a></td>
  </tr>
  <tr>
    <td><i>Job</i></td>
    <td>a paid position of employment</td>
    <td><a href="https://boards.greenhouse.io/prismatic/jobs/46953">example</a></td>
  </tr>
  <tr>
    <td><i>Event</i></td>
    <td>tickets for purchase to a show, concert, or other event</td>
    <td><a href="http://www.eventbrite.com/e/big-band-first-fridays-tickets-15692481635">example</a></td>
  </tr>
</table>


Each of the preceding subdivisions also contain the subclass *Other* that is
applied to all webpages that do not fall into one of the aforementioned sets.

The top-level *flag_nsfw* attribute partitions webpages into those that are
safe for work and those that are not. Those that are not safe for work are
divided into *Porn*, *Softcore*, and *Risque*. *Porn* applies to content that
contains nudity published by the sex industry. *Softcore* pertains to articles
that are not *Porn* but whose primary focus is imagery that objectifies people
in sexual ways. *Risque* is for content that is sexually suggestive, but not
covered by the first two categories. Note: at the moment, content
classification for NSFW aspects is determined solely based on the text and
metadata of the web page -- not the imagery.

## How do I tag URLs with aspects?

### Get a Token

If you haven’t already, [sign up for an Interest Graph API token](http://interest-graph.getprismatic.com/). 

### Programmatically Tag a URL

To programmatically tag a URL, post a request to [http://interest-graph.getprismatic.com/url/aspect](http://interest-graph.getprismatic.com/url/aspect) as demonstrated in the following curl request:

`curl -H "X-API-TOKEN: <API-TOKEN>" 'http://interest-graph.getprismatic.com/url/aspect' --data 'url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMachine_learning'`

### Analyze the Response

Here is an example JSON response from the aspect endpoint:

 <table style="width:400px;">
    <tr>
          <th>JSON response</th>
    </tr>
    <tr>
          <td><img src="/content/images/2015/03/image_2.png"></td>
    </tr>
</table>

In the response map, there is an `aspects` key containing a map of aspects for the query. The aspects map recursively specifies the path from the root of the Aspect Hierarchy to the most specific aspect relevant to the query. The key in the aspects map corresponds to the attribute key, and the value is another map containing the value for the attribute under the `value` key, the score for the aspect under the `score` key -- representing the degree of confidence that we believe the article matches the aspect at that level. If applicable, there is also a map of `sub-aspects` which contain predictions for more specific aspects that appear below the general aspect. This `sub-aspects` map is recursively structured similar to the top-level aspects map.

In the example, the response contains predictions for two levels of the Aspect Hierarchy: the *type*=*Article* aspect and the *content*=*Interview* aspect, which is nested under the *Article* aspect. The example shows that the score associated with the *Article* aspect is 0.99846, 

whereas the score associated with the further refinement is the slightly weaker 0.94315. The example also shows that the article has attribute *flag_nsfw=false* with 100% confidence.

For convenience, we’ve also included an endpoint for tagging URLs with both topics and aspects:

`curl -H "X-API-TOKEN: <API-TOKEN>" 'http://interest-graph.getprismatic.com/url' --data 'url=http%3A%2F%2Fen.wikipedia.org%2Fwiki%2FMachine_learning'`

## Send Feedback 

Our goal is to make the Interest Graph as useful as possible, and that works best with your input. Let us know if there are aspects that you would like to see added. Think you know anyone whose business might drive incremental revenue through better models powered by our APIs? We’d love to learn about the use case and brainstorm application ideas. Get in touch at [public-api@getprismatic.com](mailto:public-api@getprismatic.com).

