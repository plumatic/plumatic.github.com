---
layout: post
title: 'List Beats Grid: Linear Feeds Perform Two to Three Times Better Than Grids'
date: 2014-03-31 23:49:59.000000000 -07:00
---
Many content-centric products have been moving to a magazine or card style layout, popularized by apps like Flipboard and Pinterest. Both Flipboard and Pinterest are beautifully designed products, but our studies indicate multi-item-per-row grid layouts deliver inferior results to single-item-per-row list layouts for our particular design problem. These relative results may be unique to us, but we nevertheless publish them here in case they may be relevant to others considering the tradeoffs involved in list vs. grid designs.

In early 2013, we began a complete redesign of Prismatic together with Teehan+Lax, who just [shared their story](http://www.teehanlax.com/story/prismatic/) about the experience. We explored Prismatic at a conceptual level, and wanted to iterate very fast, so we chose to do so on web, and ultimately shipped the new version of the product with a grid layout. We got a lot right in the process, but we left some good things behind about the old Prismatic. [Today we’re abandoning the grid layout in favor of reverting to a list](http://preview.getprismatic.com).

For the purposes of this post, when we talk about a grid, we are referring to multiple content items per row, not baseline grids, which we always use for layouts.

Here’s what web looked like before embarking on the redesign.

![](/content/images/2014/Apr/old-web.jpg)

Since we wanted to experiment without forcing all our existing customers to live through a rapid and peculiar sequence of changes, we put the new grid design up on preview.getprismatic.com

![](/content/images/2014/Apr/preview-grid.png)

Teehan+Lax has [written up](http://www.teehanlax.com/story/prismatic/) the detailed design process that lead to all aspects of the grid design, nav and other screens in Prismatic, so we won’t do that here.

Our metrics on this redesign were generally better than old web, but none of us at Teehan+Lax or Prismatic was certain we were really on the right path. We had executed a grid-based card style syout about as good as it was going to get, but it had a lot of remaining issues that were difficult to reconcile.

# Motivations for list

The market for screen sizes is trending towards the small. [According to data released by IDC](http://www.icharts.net/chartchannel/worldwide-tablet-screen-size-forecast-3q-2013-share-units_m33bzy5fc#), the market for tablets with smaller screens (7-8 inches) is expected to dwarf the market for larger screen sizes (8-11 inches). According to the report, by 2017 small tablets will account for 230 million shipments, compared to larger tablets, which will account for only 135 million shipments. On top of this, the combined mobile phone and tablet markets will continue to dwarf the larger-screened desktop market, [according to a Gartner research report](http://www.gartner.com/newsroom/id/2645115).

List view is better for phones and smaller tablets because good type design implies a single column to achieve adequate font size and line length. Designing a magazine or card based grid for larger tablets or desktop is a lot of extra work for platforms that are not as rapidly growing in market share. We wondered if these grid layouts yield enough marginal performance gain on larger screens to offset the cost of building and maintaining them.

A list layout for all screen sizes is beneficial for us due to the dramatic simplification of design and engineering problems associated with our layout engine and the tangential benefits extending from this simplification. A list view is easier to make responsive on all screen sizes and provides a coherent experience across web and native apps on all devices.

One of the biggest advantages of a list view is the ease of gathering proper data for relevance training, which is key to Prismatic's powerful relevance engine. The list view allows us to present stories in the sorted order our ranker intends, and learn from how people interact with stories based on their order in the feed. A list view also affords more flexibility in displaying comments and other social context - you do not need to worry about harming scanability due to row misalignment of headlines and other type in neighboring items on the same row.

If we can converge on a list view for all screen sizes, it is strategically valuable to our design and engineering teams because we can focus on our unique value proposition: providing an extremely interesting single feed for all your interests. Rather than introducing layout complexity with low marginal value, we can spend time on information architecture innovation that helps our users find more interesting content.

We established that list works better on smaller screens due to type design constraints, so the big remaining issue is whether list view significantly underperforms grid view on larger screen sizes. If so by how much? Is the performance gain worth the implementation and maintenance cost?

# Testing list vs. grid

Before investing time in designing a universal list layout for all screen sizes, we wanted to be sure we’d see at least a neutral impact on key metrics such as story actions (likes, dislikes, saves, comments, and shares) and average visit duration.

We’d like to do this as quickly and cheaply as possible, isolating the change to ensure we aren’t confounding variables by introducing too many concurrent design changes. Preview web’s grid view had three grid units per row, so if we merged the items in each row to form a three column list item, we could test a list view. This would be a mediocre list view design, but if it outperformed the grid, then hopefully a more thoughtfully designed list view would as well.

![](/content/images/2014/Apr/preview-list.png)

Before looking into the methodology and results, recall during this testing phase we have three versions of the web product running in parallel: 1) the old list view (old web), 2) the preview grid view (preview grid), and 3) the preview list view (preview list). Preview grid is the version of Prismatic we worked on with Teehan + Lax.

![](/content/images/2014/Apr/comparison.jpg)

Importantly, there was a stalwart group of Prismatic users continuing to use old web, entrenched in their desire for a list view. We wanted to test how the preview list design performed against both users of preview grid and users of old web.

After surveying these users, we knew some didn’t move from the old web to the preview grid because they didn’t like the grid. There were a number of other aspects of the design they didn’t like, so we were not sure how much of their resistance to move was the grid style vs. other issues.

The data indicated an increase in story actions per daily active user (DAU) on preview grid vs old web, but again, we couldn't tell if this was a result of the grid design or other changes we’d made in introducing preview.

<table style="width:800px">
	<tr>
  		<th>Metric</th>
  		<th>Increase on Preview Grid vs. Old Web</th> 
	</tr>
	<tr>
  		<td>Story Views per DAU</td>
        <td>1.31</td>
	</tr>
    <tr>
  		<td>Clicks per DAU</td>
        <td>1.06</td>
	</tr>
    <tr>
  		<td>Dislikes per DAU</td>
        <td>0.50</td>
	</tr>
    <tr>
  		<td>Saves per DAU</td>
        <td>2.00</td>
	</tr>
    <tr>
  		<td>Likes per DAU</td>
		<td>1.50</td>
	</tr>
</table>

Our first step was to introduce the preview list design to 20% of users on preview grid on January 24.

To elicit feedback from old web users on preview list we emailed a sample of users who had logged into old web 15 of the previous 30 days. We received 68 responses to our survey indicating that more than half of the stalwart old web users would now be willing to switch to this new list view.

![](/content/images/2014/Apr/survey-screenshot-1.png)

To our surprise, when evaluating the numbers from the 20% of users on preview grid who had been switched over to preview list, the results were significantly better. We wondered if this might be due to a novelty bias, and so we waited between rolling each new cohort on preview grid into preview list. We increased the proportion of users on preview list by around 20% every week or two until all users were on preview list. The performance gains held stable over many weeks and remain stable today.

<table style="width:800px">
	<tr>
  		<th>Metric</th>
  		<th>Increase as result of linear</th> 
	</tr>
	<tr>
  		<td>Likes</td>
        <td>2.97</td>
	</tr>
    <tr>
  		<td>Dislikes</td>
        <td>3.08</td>
	</tr>
    <tr>
  		<td>Interest Follow</td>
        <td>2.18</td>
	</tr>
    <tr>
  		<td>Save</td>
        <td>2.72</td>
	</tr>
    <tr>
  		<td>Share</td>
		<td>2.67</td>
	</tr>
    <tr>
  		<td>Comment</td>
        <td>4.54</td>
	</tr>
    <tr>
  		<td>Average Visit Duration</td>
        <td>1.54</td>
	</tr>
</table>

These performance gains made us confident that designing a universal list view across all screens would be a sound investment.

# Feed redesign process
To start the redesign, we gathered input from our team, old web users, preview users, our [iOS app](https://itunes.apple.com/us/app/prismatic-always-interesting/id551206444), and our metrics across all these different versions of the product. We have strengths and weaknesses in all our current production designs, so we wanted to keep the best parts of old web, preview web, and our [iOS app](https://itunes.apple.com/us/app/prismatic-always-interesting/id551206444).

We started by sending out an email internally, asking the team to make notes about what they liked about old web that isn’t on preview web. We found the most desirable aspects of the old web design among the team were: list view with clear big headlines and full width images, more topic tags on stories for exploring, and quick access to interests.

We also surveyed a sample of users who tried preview web and reverted back to old web, receiving 66 responses. We found the most desirable aspects of the old web design among users were: a cleaner and more scannable list view, better story layouts with bigger headlines, bigger images, and a better balance of text and images, better navigation including quicker access to interests, better responsiveness for mobile, and faster load times.

Given the surveys, metrics, and our past feed design experience, we laid out our goals for the feed redesign.

* Design great story layouts with big headlines, big images, and a good balance between text and images.
* Design a universal list layout that works great on all screen sizes from big desktop screens all the way down to mobile, then use those same layouts for our iOS and Android tablet and phone apps - including both portrait and landscape.
* Design a feed and layout system that accommodates social context such as our explain system (to be explained momentarily) and comments, as well as non-article story units like interstitials and rich removes (to be explained momentarily).
* Design an explain system that enables discovery of topics you don’t follow and helps the customer understand how Prismatic works and why things show up in their feed.

In the past we’ve made the mistake of designing one platform at a time, which yields excessive churn as you discover new things on each platform and loop back to redesign the previous platforms - designing universally with screen characteristics rather than ‘mobile first’ was critical for us this time around. Mobile first creates churn, now we design ‘universal first,’ aiming to develop modular systems that maximize the experience on each screen, rather that churning as we jump from to screen to screen always yearning to redesign the last screen after finishing the current screen. Here’s a look at the results of this process.

![](/content/images/2014/Apr/prior-comparison-2.jpg)

This time around, we’ve designed a system that works well on all screens. We looked at a few important points. Designing for Android, which means things need to be flexible within ranges of screen sizes due to the vast number of screen dimensions - even if you have some discrete settings, you can’t make everything absolute as is the case with iOS. Designing for mobile, especially tablets, implies thinking about both portrait and landscape, and you hopefully want to think about reusing portrait layouts on larger screens as landscape layouts on smaller screens. We had to create unique layouts for phone portrait and phone landscape, but we were able to reuse web small as tablet landscape. So in the end, we have 5 discrete settings; phone portrait, phone landscape, tablet portrait, tablet landscape / web small, web large. The only setting that differences in content layout rules is that on phone portrait, we do not allow two columns, so images can not appear to the right of body copy.

You can see the first iteration universal feed layout in production [here](http://preview.getprismatic.com), and a look at how it responds to different screen sizes below.

![](/content/images/2014/Apr/tablet-mobile-feed.png)
![](/content/images/2014/Apr/wideweb-laptop.png)

We also intended to design our universal feed to accommodate what we can interstitial and rich remove story units. These are examples of non-article story units that can cause some problems in some settings, but work great in our new list.

Interstitials are a way of visually calling attention to a story unit other than a typical article type, for example, if we want to suggest some topics we think you’ll be especially interested in. We want to ensure that we design a feed system with interstitials in mind. The great thing about list layouts are their inherent row-based flexibility. Since there is nothing to the left or right of each unit to align with, it is hard for the system to fall apart when you introduce new components like interstitials.

![](/content/images/2014/Apr/interstitial.png)

Rich remove is a feature that allows customers to help us improve the quality of results they get. When a customer dislikes a story with thumbs-down, we can overlay the ‘rich remove’ model on the story, which allows the customer to block topics, publishers, or people, or tag a story as having certain undesirable attributes.

![](/content/images/2014/Apr/rich-remove.jpg)

With our universal list accommodating all screens and story unit types, we were ready to test it out against our recent feed design for [iOS](https://itunes.apple.com/us/app/prismatic-always-interesting/id551206444) that we shipped in late December 2013. We decided to test this on an iPhone against our new universal feed layout, also on an iPhone. We did a round of internal critique with everyone in our company, comparing our latest iOS feed design with our new universal feed design. Then we did a think-aloud study to see what people thought upon seeing both our latest iOS feed and new universal feed design. 

![](/content/images/2014/Apr/current-vs-new-feed.png)

The feedback from the internal critique was that the new feed looks better aesthetically, the topic tags are cool, the bigger images are great, the separation of stories was unclear, and some of the explains were unclear.

The feedback from talk-alouds done with folks we found at the mall and out on the street was that the new feed seems to be clearer and have better aesthetics, the tags are cool, the new explain system is better but some of them are confusing, and separation between stories is unclear.

The biggest issue we found with the new feed design was that it became difficult to differentiate among stories. The interviews helped inspire a new directions to eliminate this issue. We generated and tested fifty variations total (an upcoming post details how we tested all variations on Mechanical Turk, which we’ve been using successfully for usability testing). The end result of all this was that the full bleed image story unit with icons overlaid on the image causes confusion because the image abuts the story unit below and makes it difficult for the user to differentiate which headline the story is attached to - we solved this by eliminating this approach and removing any metadata or action icons that had been overlaid on images, and giving all stories the same number of baselines of space at the bottom before the next story starts.

The next biggest issue was confusion with topic tags and explains, so we designed more specific experiments using the topic tag and explains options below.

*Topic Tags*

![](/content/images/2014/Apr/tags.jpg)

*Explains*

![](/content/images/2014/Apr/explains.png)

After generating a few options, we surveyed the team to get feedback on the design of topic tags and explains. We found unanimous preference for adding a check next to the topics in tags to show you follow them as opposed to changing the colors of tags or adding the stroke border. Explains were generally well received, but we found some confusion with the ‘hot’ explains, as well as authority explains.

We also posted a survey on Mechanical Turk asking respondents to comment on what they thought the explains meant. The majority of the explains we tested were understandable, but again we found confusion with both ‘hot’ explains and authority explains.

We decided to ship with the check model of indicating following on topic tags, and a limited set of explains that avoid the ones that caused confusion. We intend to revisit a more diverse set of explains later after doing some testing with the initial limited set that include just first degree connections and social action counts.

We’re happy with where [we’ve ended up with the new feed layouts](http://preview.getprismatic.com). It brings together the best of what we’ve done across different generations of design on both web and [iOS](https://itunes.apple.com/us/app/prismatic-always-interesting/id551206444). The design is fully responsive and works great on mobile. There are a number of issues, but we know we’re following a path that’s validated by metrics and user studies, and we think this is a foundation we can build upon in small increments rather than big redesigns. Designing a simpler and more universal system can be better for you, and better for your customers, so move carefully and with data before jumping into big redesigns with more complex layouts.

![](/content/images/2014/Apr/laptop.jpg)
