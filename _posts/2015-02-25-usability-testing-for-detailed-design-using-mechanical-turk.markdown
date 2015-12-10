---
layout: post
title: Usability Testing for Detailed Design Using Mechanical Turk
date: 2015-02-25 10:54:29.000000000 -08:00
---
In designing story units for our new home feed, we knew subtle variations could make a big difference. To test different variations, we started with in-person interviews. Once we realized that we had biased the results with the ordering of story units in the mocks, it became clear that we’d need to test too many variations to make in-person interviewing a feasible method.  These complications make it tempting to forgo testing entirely, or to just build different variations and A/B test in production. While this production feedback loop is important, it’s also expensive to build and A/B test a large numbers of different options. The more we can narrow the decision space with sound research methods, the better.

To help with this problem, we started exploring remote testing as a way of getting diverse feedback quickly for detailed design in particular. We find this works well because for detailed design we aren’t looking for the rich insights derived from the in-person interviews. All we need is a reaction, letting us know if various pieces of information can be comprehended at first glance. We’ve found Mechanical Turk, an online task marketplace, to be an effective tool for crowdsourcing usability studies. In this post, we’ll share how we incorporated this tool into testing the redesign of Prismatic’s home feed and explain the critical nuances involved in getting good results using Mechanical Turk.

**Testing Home feed**

The home feed is where users spend the majority of their time on Prismatic. The proposed home feed below is the result of lessons learned from previous designs and explorations from our internal hack week. We decided to test our current production iOS home feed against the proposed home feed with some in-person interviews.

![image alt text](/content/images/2015/02/image_0--1-.png)

After a round of in-person interviews we found a number of issues, the largest being difficulty differentiating among stories in the feed - an important factor in making the feed easy to scan. Three story units were prone to confusion: the full bleed image unit, gallery unit, and comment unit.

![image alt text](/content/images/2015/02/image_1-1.png)

We had done our initial in-person testing with static mockups without randomizing the order of different types of story units, so it’s possible the confusion could have been caused by the specific order in which the story units appeared. This is especially problematic because comment units always appeared under full bleed image with action overlay units. We’d designed a poor experiment and now had to determine a way to disambiguate and hone in on the real problems that needed to be redesigned.

To start with, we needed to test variations in order to distinguish which particular story units or orderings among story units were causing the confusion. We ended up with over fifty variations of story designs, which would mean we’d need to do at least 250 interviews to test the variations, and twice that for additional validity in accordance with [what researchers typically recommend for usability studies](http://www.nngroup.com/articles/why-you-only-need-to-test-with-5-users/). This made it infeasible for us to invest the time in exploring this issue with in-person interviews. We could remove options through subjective assessment given we knew some were poor. However, these are important variations to test as controls. If results reveal the options we know to be poor to be superior, the validity of the study would become questionable.

We also needed a diverse group of respondents that represented the wide demographic of users. Feedback from users who aren’t familiar with the product would be particularly important as we’ve found the feed itself is an important way of learning to use the product.

We designed a task to test whether people understood the boundaries between story units: ask people to tap on the story they thought the comment in question belonged to. We used a heat map with defined click areas to evaluate the performance of each variation. 

**Screening Turkers for high quality participants**

We decided to try Mechanical Turk to run this experiment because it’s inexpensive, fast, and [representative of the general population](http://www.npr.org/blogs/alltechconsidered/2014/03/05/279669610/post-a-survey-on-mechanical-turk-and-watch-the-results-roll-in), including many people who have never seen Prismatic before.

Despite these benefits, the platform has bots and ‘distracted’ users - a person who glances over questions to quickly answer them, often misunderstanding the question in the process. If we were to use Mechanical Turk, we’d have to find a way to filter responses from these turkers.

To remove bots and distracted users from our study, we created a screener. The screener had to be such that if you got it wrong, you’re likely a bot, not paying attention, or otherwise incapable of participating in the experiment in a productive way. It’s important the screener pose a question associated with the task at hand to establish a baseline for task comprehension; in this case the task was reading and interacting with articles. With these criteria in mind, we decided to use a screenshot of a New York Times (NYT) page under the premise that if someone can understand the NYT page and find the comments link on that page, then they ought to be able to find comments on the Prismatic feed. 

To isolate issues associated with the screenshot of the NYT page from the rest of the study, we tested the screener on its own. If the majority of turkers taking the survey don’t pass the screener, there may be a problem with the screener itself. 

The screenshot below shows the prompt turkers were asked to respond to.

![image alt text](/content/images/2015/02/image_2.jpg)

After a couple hours, we had 52 people complete the screener. Below is a heat map summarizing the results of the pilot screener.

![image alt text](/content/images/2015/02/image_3.png)

50% of turkers responded correctly, clicking within the region we specified around the comments link for the article. Of the incorrect responses, 25% of clicked within the article unit, mostly around the article title.

To actively deter responses we’d have to remove from our study, we sought to find patterns amongst the incorrect respondents. In investigating the demographics of people who responded, we found about half were located in India, and two-thirds of the answers provided by this subgroup were incorrect, so we removed India from our study population.

The most important lesson we learned from launching the pilot screener is to expect a significant portion of responses to be unreliable, and know that you may be able to identify and screen out systematic biases. Since 50% of screener respondents answered incorrectly, we aimed to double the number of responses we were originally looking for in order to meet our population size goals. 

**Main Study**

As mentioned above, we had 3 story units in particular we wanted to test: full bleed image unit, gallery unit, and comment unit. In order to create the variations, we systematically altered the story unit’s side-margin (margin to the left and right of a story) and story action placement (whether the actions were overlaid on the story or placed below). As an example, the four variations of the gallery story unit are below.

![image alt text](/content/images/2015/02/image_4--1-.png)

We also wanted to test whether order placement was confusing. We have five total story units: the three previously mentioned (full image, gallery and comment), in addition to a text only unit and text-and-photo unit. The image below shows the ‘side margin-actions below’ gallery variation atop all 5 story units.

![image alt text](/content/images/2015/02/image_5.png)

To test the gallery unit in all cases, we would have to test all four side margin and action placement variations atop all 5 story units, leading to twenty variations. We had twenty more variations for the full image unit and ten for the comment unit leading to the fifty total variations.

For each of the configurations we tested, we showed the user a screenshot of the story unit in a feed between two other stories, and gave the user a task related to that story unit. For example, for the comment story unit, we showed the following question and screenshot:

![image alt text](/content/images/2015/02/image_6.png)

In order to reduce cognitive load, get faster results, and prevent bias, we randomly presented each user with only one feed variation and randomized the order of the screener question and feed question.

In total, we had over 700 responses. After removing the responses of the users that had incorrect answers to the screener question, 67% percent of the original responses remained. It is worth noting that after adjusting our population from the pilot study to remove India, we were able to improve our valid response rate to the screener from 50% to 67%. In order to analyze the data in aggregate, we created a visualization.

The visualization below, laid out as a matrix, presents our results for the gallery story unit. 

The columns represent the variations in side-margin and action overlay, while each row is a different story unit that the gallery variation is presented atop of. Each cell in the matrix placed a gallery variation above different types of content. We were interested in seeing if users correctly associated the comment section with the gallery above. The percentage under each of the 20 permutations represents the percent of people that clicked in the correct region, with the ‘n’ being the number of people who were presented with that variation in the survey. And finally, the large percentages at the bottom of each column are an average of the ‘success’ rates for the side margin/action placement permutation when paired with each of the story units. 

![image alt text](/content/images/2015/02/image_7.png)

The ‘side-margin actions-outside’ variation of the gallery unit was most successful, with 75% of respondents clicking the correct area. In general, we found margin and actions below the image yielded greater usability across all variations. You can see this design [in production](http://preview.getprismatic.com/news/home).

The poorest performing result was the variation with no margins and actions overlaid on full-bleed images. In the in-person research, we’d conflated this full-bleed image unit with the comment unit by always placing it above the comment unit, and we got feedback about confusion regarding both the comment unit and how to distinguish separation between stories. We could not be sure that the comment units and all other units were fine, and full bleed image with actions overlaid and no bottom margin was the entire problem. This study using Mechanical Turk allowed us to understand that the full-bleed image and gallery layouts were the only problem, and select a layout that maximizes image sizes within the confines of keeping things understandable.

This experiment has changed the way we think about remote usability testing. We’ve always tried to match the research method to the problem, but thus far our only remote experiments had been surveys. We were looking at a solution space where we knew subtle variations could make a big difference and in-person testing would be too time consuming. Surveys wouldn’t be realistic because they would requiring users to observe a mock, then reflect to answer questions in text, rather than interacting with the mock directly. We now understand how to use Mechanical Turk effectively to test direct interaction, which gives us another powerful tool for detailed usability experiments which would otherwise be prohibitively costly to run. These experiments saved us a lot of time since we avoided the churn of building and A/B testing an excessive number of design permutations in production.

[Learn more](http://getprismatic.com/team) and [keep up with](http://www.twitter.com/prismaticdesign)  Prismatic Design.
