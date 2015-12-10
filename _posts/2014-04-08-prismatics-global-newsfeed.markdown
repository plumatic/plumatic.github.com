---
layout: post
title: Prismatic's Global Newsfeed
date: 2014-04-08 13:27:14.000000000 -07:00
---
One of our users' favorite things about Prismatic is that it's highly targeted -- personally, I love surfing topic feeds about robots, San Francisco food, and Clojure -- but they also want to keep up on the most important events happening around the world, across a much wider range of topics.

That's the purpose of our Global feed, which we're making public today on the heels of opening up hundreds of thousands of interest feeds on Wednesday. This post will explain the vibe of our Global feed, how it differs from other "Top News" sites like Reddit or Google News, and how we arrived at its current implementation.

## What's Popular?

A first idea to single out important events is to sort recent stories by the total number of times they have been shared on social networks like Twitter. This was the basic idea behind an earlier iteration of our Global feed.

With a quick glimpse at the data, however, the problems with this approach become obvious. Of the twenty most-shared articles on Twitter over the past few days, thirteen are about social media or technology, and seven are from a single publisher (here are three representative examples). While social media, technology, and funny videos (biting social commentary notwithstanding) are undoubtedly important parts of the zeitgeist, when more than a hundred such stories appear before any mention of important global events we have clearly failed to capture the perspective we're aiming for.

Thus, while shares on sites such as Twitter are a powerful source of information about the content people are enjoying and engaging with across the internet, simply picking the most-shared articles has an unacceptable bias towards tweeting cats. To single out stories that nobody wants to miss about the most important events happening around the world, we had to look for other signals.

## Who's Writing About It?

Counting shares of individual articles ignores an key fact: when a truly important event happens, there are usually lots of different articles written about it. Perhaps the best-known embodiment of this idea is Google News, which collects, organizes, and displays thousands of articles (primarily from traditional publishers) about each newsworthy event.

Of course, to be able to put this idea to work ourselves we need to know more than just which articles are out there and who is sharing them: we also need to know what each article is about, so that we can cluster together all of the articles about a single event. This is a technically difficult problem; fortunately, we have recently implemented a system that solves it quite well.

After developing this system, the next Global feed experiment we tried was to sort the story clusters, preferring those with the greatest number of articles about the same event. While this was a definite improvement over pure social sorting, it still didn't quite have the right feel. There were still too many tech stories, and some of the important and highly shared but more unique stories were lost under the news wire.

## Our 'Global Feed'

After investigating social- and aggregation-based ranking independently, we were pretty sure that the two could be combined to yield a really great Global feed. Now, it was just a matter of figuring out how to best combine the signals to produce the vibe we were looking for.

Getting this right took a lot of experimentation, but we finally settled on a simple scheme that seems to work quite well. First, we only consider events with at least 5 stories about them as eligible for inclusion the Global feed. Then, we rank all of the stories about such events as usual, but give a small bonus to stories with many related articles with lots of shares.

This allows the content to speak for itself, and relies on our stock ranking pipeline to produce a final feed consisting of high-quality stories about important events, chosen from a diverse set of publishers and topics. Moreover, like the rest of Prismatic, it only gets better if you sign in -- you'll see more stories by publishers and about topics you care about, comments by people you know, and more.

Check out the Global feed, and please let us know what you think by filling out this brief survey.
