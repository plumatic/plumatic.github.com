---
layout: post
title: American Healthcare Fraud and Scalable Investigative Reporting
date: 2014-04-08 13:31:44.000000000 -07:00
---
Depending on estimates, [medicare fraud](http://en.wikipedia.org/wiki/Medicare_fraud) costs the US government between 20 and 100 billion dollars a year.  Last week, [reports came out](http://www.latimes.com/health/la-na-medicare-fraud-20120503,0,2331593.story) that 107 doctors, nurses, and social workers across the country were charged in a nationwide sting on Medicare fraud totaling about $452 million dollars, and the Department of Health and Human Services stopped payments to 52 medical providers.  Data mining lead to the largest healthcare fraud takedown in American history.

At Prismatic, we’re very interested in the intersection of data and news.  We recently got the chance to moderate a session at the CIR Techracking conference on  [Decoding Prime](http://californiawatch.org/prime) - which takes a look at government data with some some good investigative reporting to [uncover hospital medicare fraud](http://californiawatch.org/dailyreport/precision-journalism-reveals-patterns-government-data-14117).

Decoding Prime and the big $452MM sting operation announced last week are both great examples of a proof of concept for how data mining and investigative reporting tactics can drive transparency in the healthcare business.

But the big challenge is scalability.  There is a lot of friction in obtaining data from the government, so transparency is limited.  And we need to be able to leverage data and technology to make journalists and investigators more productive.  The $452 MM sting is great, but it may be less than 1% of the fraud that occurs in a single year.  Without easier access to the right data, and smarter processes to take advantage of that data, reporters will be unable to uncover enough breadth of abuse to drive full scale transparency.

###Transparency requires open data

While researching to prepare for the session, it became clear that there is a correspondence between the volume of healthcare fraud in the United States and the friction involved in getting the data required to investigate potential fraud.  Consider a [recent example](http://online.wsj.com/article/SB10001424052748704696304575538112856615900.html) from the Wall Street Journal.

The Wall Street Journal attempted to access the Medicare claims database for almost a year, and the government attempted to charge them $100,000.  The WSJ ended up joining with the Center for Public Integrity to file a lawsuit against the Department of Health and Human services and obtaining the data at a reduced rate in a settlement.

We can’t expect results when there is this much friction just to get access to data.  If we want transparency from government and industry, then we need freely available open data.

###Scalable investigative reporting with data and tech

Both the recent sting and the Prime case show that you need real journalists and investigators working with technology and data to achieve good results.  The challenge now is to scale this recipe and force transparency on a larger scale.

We need to get more technically sophisticated and start analysing the data sets up front to discover the right questions to ask, not just the answer the questions we already know to ask based on up-front human investigation.  If we have to discover each fraud ring or singleton abuse as a one-off case, we’ll never be able to wipe out fraud on a large enough scale to matter.

### Getting leverage from the community

There are a lot more possibilities to leverage the power of community for investigative reporting.  We can invite the audience to participate as in an example pointed out to us by [Steve Doig](http://cronkite.asu.edu/faculty/doigbio.php), Knight Chair in Journalism at ASU.  In this [Pulitzer winning](http://www.pulitzer.org/bycat/Public-Service) work from the Philadelphia Inquirer in 1999 uncovering [systematic under-reporting of rape cases](http://inquirer.philly.com/packages/crime/html/sch101799.asp), crowdsourcing allowed women to look at how their case was reported, and write in to inform the Philly Inquirer if their case had been mis-reported.

This kind of audience participation isn’t always possible, because people may not know they’ve been taken advantage of or have the visibility to find out.  However, there are lots of other opportunities for creative use of community - for example opening up datasets for exploration by talented citizenry, and driving better distribution of investigative reporting by adjusting the format from strictly long form to more interactive campaigns composed of stories and other ways of engaging the community.

## An interview with Chase Davis about the Center For Investigative Reporting as a lab for the newsroom of the future

After TechRacking, we followed up with Chase Davis from CIR, and found out that CIR is in finalizing a [merger with the Bay Citizen.](http://cironline.org/blog/post/center-investigative-reporting-merging-bay-citizen)  According to Davis, the newly merged CIR and Bay Citizen team will have a team of 10 engineers and researchers - making them one of the 2-3 largest journalism/technology teams in the country.  This blend of technologists and journalists could be an experiment for the data-driven newsroom of the future.

*1.  With the CIR team expanding from two researchers and a developer, to one of the largest tech-journalism teams in the country, what are some of the first things you want to tackle, and why will you be able to tackle things now that you couldn't before?*

We've actually been very lucky about the way this team has come together in terms of complementary strengths. CIR has done a very good job on the data journalism side of things for the past few years, but we've been weaker in the areas of core engineering, product development and design. The Bay Citizen's team is largely focused on those areas, and they're very, very good.

When you bring those two sides together, you end up with the ability to do a lot of things. There's the journalism, of course, but we also hope to pursue product development opportunities around open data; tackle some problems around very stubborn problem areas in journalism, like engagement, metrics and mobile; and open source a hell of a lot of code. We don't see ourselves as just a CIR/Bay Citizen technology team -- we think we have a responsibility to create generalized solutions to problems that can scale across the industry.

*2.  How will you blend interactivity with the audience?  Will it just be long-form, or trickle of smaller nuggets as well?  Will you try some of the social hacks for audience participation we talked about during the panel or do you think that's not that useful?*

One of the great things about this combined organization is that we're actually planning to have an entire team devoted to community engagement. We've had one public engagement manager, Ashley Alvarado, working with us for a while now, and she's come up with all sorts of creative ways to reach underserved communities our stories and to engage them in the reporting process. You may or may not be familiar with the Public Insight Network (http://www.publicinsightnetwork.org/), but we've gotten a lot of mileage out of that over the last couple years.

In terms of the technology/journalism component of crowdsourcing, I think it can produce amazing results but you have to pick your battles. The Guardian in London did a great project a few years back where it crowdsourced expense reports from members of Parliament and basically asked people to point out stuff they thought was interesting (http://mps-expenses.guardian.co.uk/). The system had to be designed in such a way that it minimized friction and actually made people want to contribute. They got great results, but they also put a lot of effort into choosing the right project, building the right system, maximizing engagement and minimizing the potential for error. We absolutely plan to pursue those projects, but we have to pick the right opportunities.

*3.  you mentioned doing more with the data up-front to get more leverage for the real grunt work investigative part of the job, give me a few of your top ideas on that.*

Here's a good example. I used to cover politics, and a big part of that is reporting on candidate campaign finance reports -- how much money they raised, from whom, all that good stuff. Frankly, so many of the stories and analyses around campaign finance bore me to death. So they're just simple sorts -- who gave the most, who gave the least. Good reporters can find interesting stories behind those numbers, but I think we can find more interesting numbers to start with.

So as part of a project we're working on now, we're trying to build a classifier that will detect "interesting" campaign contributions by a given donor. We're still defining exactly what "interesting" means, but in general you can look at it as donations that deviate from a donor's typical pattern. Are they giving to members of a party they don't usually support? In amounts that are unusually large or small? At times of the year where giving doesn't usually take place? Things like that. If we can model "interesting" in this context, we can create an early warning system that shows us when donors are behaving outside their normal patterns -- and that kind of behavior is often newsworthy.
