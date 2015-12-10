---
layout: post
title: Announcing an All New Prismatic for Web
date: 2014-04-08 12:13:55.000000000 -07:00
---
Since we launched Prismatic in closed beta this April, we’ve [opened](http://blog.getprismatic.com/blog/2012/6/20/prismatic-launches-the-most-interesting-place-in-the-univers.html) our topic and publisher feeds to the public, launched the [iPhone app](https://itunes.apple.com/us/app/prismatic-always-interesting/id551206444?mt=8) (and updated it for the iPhone 5), and [introduced](http://blog.getprismatic.com/blog/2012/9/17/announcing-following-people-are-more-interesting-on-prismati.html) social on the web. Through it all, the web UI has fallen into disrepair.  We started redesigning the web by reading user feedback and doing new user research. Check out the [results](http://getprismatic.com/). It’s not perfect, but it’s a reasonable foundation.  The rest of this post is for the design lovers out there that want to hear about the redesign process.
<br>
<img src="https://dl.dropbox.com/s/qeol4las3s6tmwg/1.article-layout-before.jpg?dl=1">
<img src="https://dl.dropbox.com/s/wugne5mb3ws58j0/2.article-layout-after.jpg?dl=1">

# User Research and what we learned #

As we did during the [iPhone design process](http://www.quora.com/Prismatic/What-key-decisions-influenced-Prismatic-for-iPhones-interaction-design ),  we did a week or two of user interviewing interleaved between redesign sessions.  Here’s what we learned:

* Onboarding should get people to interesting stories and showing them how things work from there.
* Our sidenav was confusing and overloaded with lists.
* Social context and actions were confusing.

This mostly confirmed how we felt, and we had some additional problems:

* We didn’t have a simple and consistent color system.
* Our type felt retro-modern (1900-1950), not new-modern (post 2000).
* Headlines, divider lines and layouts were all weak.

## Layout ##


<img src="https://dl.dropbox.com/s/ul8xx4opa9iq252/3.article-layout-sections-before.jpg?dl=1">
<br>
There are three kinds of information in a Prismatic story: the story content itself (highlighted in green above), meta information (highligted in orange),  social information (also highlighted in orange).  The story actions are highlighted in blue. The purpose of the meta and social information (topics, publisher, author, and who recommended and shared it) is to give readers information about why they should read an article.  The story actions allow a user to share, recommend, save, or remove a story.
</br>
<img src="https://dl.dropbox.com/s/sxyoepdllkzn62z/4.article-layout-sections-after.jpg?dl=1">


Our new layout consolidates the social information and story actions into a single bar in the bottom right of each story; this leaves more room for content and simplifies the presentation of social information and available actions. In the old layout, we distinguished avatars depending on whether the user recommend or shared a story; that has gone away in favor of just a simple row of users who have interacted with the story. 

One confusion we discovered was that users didn’t understand when they took a public action (recommending or non-email share of a story by default) this would increase the likelihood the story showed up for their followers and other similar users. Your avatar appears in the story, signalling your interest to others. To make this connection clearer, when you recommend or share a story, your avatar is pushed onto the row (see images above). This animation serves as confirmation that the action succeeded and you’ve joined the other people who are interested in the story.  When a user you follow interacts with a story, their avatar will appear with an orange underline to help you notice them. 

<img src="https://dl.dropbox.com/s/assoziaq4qg6ozd/5.story-actions.jpg?dl=1">

Both the headers and the social context were sloppy in the prior design, so we were delighted that moving the social actions down into the single social section of each story aligned with the motivation of simplifying story headers.

We increased the emphasis on headlines and swapped their position with the meta lines, makes scanning stories easier. We’ve also started doing smart highlighting in the meta line: When a story has a topic or publisher you follow, the interest link will also appear orange.   In the story below, my interests in Architecture and Urban Planning. The orange highlighting of interests and people make it easy to see why you might find a story interesting.
 

## Typography ##

Prismatic is meant to feel modern and exciting and our old type didn’t reflect this. We we’re using [Akzidenz-Grotesk](http://en.wikipedia.org/wiki/Akzidenz-Grotesk) as our Sans-Serif, which was designed in 1896, and [Egyptienne](http://en.wikipedia.org/wiki/Egyptienne_(typeface))  as our Slab Serif, which was designed in 1956. Neither of these fonts are digital-native. 

The new design uses Unit Slab serif for headlines, Meta Serif for body, and Unit as the Sans for all meta and interest links. Typography nerds will know that both fonts are designed via the same collaborative team of [Erik Spiekermann](http://spiekermann.com/en/), [Christian Schwartz](http://en.wikipedia.org/wiki/Christian_Schwartz), and  [Kris Sowersby](http://www.fontshop.com/fonts/designer/kris_sowersby/) and intended to complement each other with exactly the vibe we’re looking for - modern precision with a humanist flare.

## Color ##

<img src="https://lh5.googleusercontent.com/fTdIlYuIrTILiRPhextjbxxQYSEpPrPuHboEt6TSSXsTQG92z16IKATlf0o5iVpTwNbAQesC7vdFu0QyaOrROP_--VKrnII7YVL3f5lXFQavUoW-Q0E">

Overall, our color palette was drab: grey with purples and blues. We decided to go all black, grey, and blue - simplifying to as few colors as we could.  Our current goal is simplicity and readability, we have bigger fish to fry with layout, nav, and social.

The headline and body fonts are darker yielding a stronger contrast against story backgrounds and improved readability.

## Navigation and Profile ##

The busy sidenav was one of the top things people found confusing about Prismatic.  It was overloaded with several collapsible lists. We moved this content to the profile and focused the sidenav on search, world news, and suggestions with a new mini feed layout that supports horizontal paging (try swiping left or right with your trackpad). 

<img src="https://lh5.googleusercontent.com/tP7GUks6J0SSqCbiBMief8Jaa4ZpEczqpD57q4Ug4KLIFF6gdywFYsQrJjxAYucDNcF5U9ovcl0qYJVO8X2BNGI0wcJvuFEFxlQrgNpd1QNou12TskE">

We’ve also made the [Prismatic interest graph](http://getprismatic.com/people_are_interesting ) way more accessible and explorable. Now, each interest feed has links in the header to top topics, publishers, and followers; clicking on each section drops down a view of these interests.  We use the same header design in your profile and when you view other people’s profiles you can access interests, followers, and people you are following.

<img src="https://lh6.googleusercontent.com/lQH42s05z34R9wuLwZJXShokpuTpBMj0Cdiaq85wE7EACfSza1szqL4zHD2ecUmuefUZRV3cIsMajB3PfvYHo9i_g99pO1G8ySCDtpnAn-_i-l423GU">

<img src="https://dl.dropbox.com/s/4h3j6n94x3pdtxh/header-interests.png?dl=1">

<img src="https://lh6.googleusercontent.com/mFI6gYmtM7onDkD5anju1g73iRvbcwQB8nhQKW5FwAhMkaaYCp3TUtnYTYIOnVPoBL9CYOt_CyGm6B7uhPBC4BTBUMsZnssxLug6wx5_7JjwfjgVkAA">
