---
layout: post
title: Making Good Design Decisions
date: 2014-05-14 18:58:09.000000000 -07:00
---
There’s a popular misconception that NASA spent millions in a failed attempt to create a space pen while the Russians just used pencils. The implication is that good design is simple in the sense that it is simplistic or obvious.

Simple design is often simple for the user but complicated for the creator. They really do use pens in space. It turns out pencils don’t perform well in space because wood and lead shavings in a zero-gravity, oxygen-rich environment can be dangerous for both fire and as debris. The true story of the space pen is that it was not a failure, wasn’t designed by NASA, and wasn’t even designed for space. The space pens’ design goals were ink that wasn’t exposed to air so it wouldn’t dry up, didn’t rely on gravity so it wouldn’t leak, and could write underwater and function at temperatures ranging from -30 to 250 degrees Fahrenheit. The pen turned out to also be well-designed for space and was renamed to the space pen.

> “The secret to the space pen is in the cartridge. It is a hermetically sealed tube containing thixotropic ink, pressurized nitrogen gas, and a tungsten carbide ballpoint tip. During development, Fisher found that while the pressurized cartridge successfully pushed ink out the tip of the pen, it also successfully leaked uncontrollably. Rather than redesign the cartridge, Fisher redesigned the ink. He developed a thixotropic ink that is a gel at rest, but turns into a liquid under pressure. Sort of like toothpaste. With this new, thicker ink, the pen didn’t leak and would only write when pressure was applied to the ballpoint. Success.” -[smithsonianmag](http://www.smithsonianmag.com/arts-culture/the-fisher-space-pen-boldly-writes-where-no-man-has-written-before-1020748/?no-ist)


Simple solutions to tricky design problems are often non-obvious, like redesigning ink to work with a pressurized cartridge. Finding such unconventional solutions is normally a messy process of exploration and tradeoffs. We found it impossible to follow the same preprogrammed design process for every design problem. We developed a clear system for making good decisions that keeps things organized and facilitates design critique without imposing a sequence of steps other than defining goals first. This approach could be interesting to other teams looking to make deliberate goal-driven design decisions.

###Goals


Design is the process of creating a solution that balances the goals of both the user and the creator. User goals include both tasks people need to get done and psychological wants. Creator goals include qualitative and quantitative goals that we call “design” and “data” goals, respectively. Explicitly stating and balancing these different goals is the most important part of design. Without goals, you’re not aiming correctly, so you have very little chance of hitting the target.

For example, the user goals for our recent [home feed redesign](http://blog.getprismatic.com/list-beats-grid-linear-feeds-perform-two-to-three-times-better-than-grids-2/) were finding relevant content, catching up with all your interests in one feed, clear scanability, the right amount of preview content, better sharing, and a sense of being on top of things and not missing out. We derived these user goals from product metrics and user studies. Our design goals included making stories engaging and fun, and designing a modular, scalable, responsive system. We also wanted to allow expanding to new types of story units in the future, and make the product more fun, understandable and interactive.  The data goals included increasing the number of story actions / DAU by X%, increasing session duration, and increasing cohort retention on a session/session or week/week basis. 

###Inputs

Inputs refer to information we view as valid for informing design decisions; we use product vision, analysis, metrics, user studies and implementation considerations. Even an assumption is a valid input into a decision, as longs as it’s explicitly stated. Agreeing on which inputs are valid and how to use them leads to productive problem solving discussions about information rather than fruitless arguments based on weakly informed personal opinions.

Each input should be regarded as having inherent strengths and weaknesses that can be used in conjunction to triangulate on good decisions. Metrics offer insight into what users are doing, and user studies can help you see why they’re doing it. Incorporating product vision helps us to focus investing our time on problems that align with the vision. Analysis gives us a sense of what prior information is available for us to work from. Technical constraints make sure we’re being realistic and pragmatic about the changes we want to make. Assumptions can simplify the decision making process and allow us to debug when they’re made explicit. 


**Vision**

Vision can be broadly defined as planning the future with imagination or wisdom. Visionaries are often described by others as ‘seeing the future’ but describe themselves as ‘seeing connections’ or ‘being curious.’

>“Creativity is just connecting things. When you ask creative people how they did something, they feel a little guilty because they didn't really do it, they just saw something. It seemed obvious to them after a while.” - Steve Jobs

>“I have no special talent. I am only passionately curious.” - Albert Einstein

In design, we talk about ‘taste,’ ‘product sense’ or ‘instinct.’ What often seems like a gut reaction, is really just slow, ongoing design synthesis playing out in your head - driven by curiosity, and fueled by accumulated knowledge and data. You’re likely to do better solving problems when you have deep knowledge of implementation details and accumulated observation, customer studies, market awareness, research.

>“Designing a product is keeping 5,000 things in your brain, these concepts, and fitting them all together in new ways; kind of continuing to push to fit them together in new and different ways to get what you want, and every day you discover something new, that is a new problem or a new opportunity, to fit these things together a little differently.” - Steve Jobs

We think of intuition not as a magical ability but just a lot of accumulated curiosity, thought, and experimentation. Looking at intuition in this way means we can explain the reasoning behind our intuitions if we think about it. For example, it’s ok to say ‘that feels weird’ during a design critique, but then you have to be able to reason through why you feel that way. This ability to explain why you think something ‘feels weird’ rather than just saying it feels weird is the difference between the designer getting useful feedback and conjecture based on emotion.

**Analysis**

Analysis is the collection of information related to a specific problem that allows you to understand the range of possible solutions. We look for solutions the market has been exposed to, what solutions are performing the best in the market, and whether there are existing standards in the market. You can also find case studies and research or analyze flows across related apps that are tackling the problem.

Working with established design standards is an issue that comes up often in analysis. If you are going to break an established standard, you need to ask yourself if you have a good reason. If the interaction in question is core to the value of your product, breaking a pattern may make sense. However, if it’s tangential to the core value, it's probably not worth it. For example, when we were designing navigation on the Prismatic iOS app, we followed a standard bottom navigation model, after analysis of navigation in popular social networking apps. We decided to focus our efforts innovating on other areas of the design. 

**Product Metrics**

Metrics improve design by helping you track real user behavior. Important feedback loops measure the effectiveness of design and help you prioritize how you invest your time.

When re-designing the home feed of our web product, we were considering moving to a list view vs. our gridview in order to be responsive and easily accommodate multiple devices. However, before investing a significant amount of time in designing a list view, we wanted to ensure our engagement numbers would be the same or better than for the gridview. To do so, we hacked together a list view and tested it on 20% of users, paying close attention to their interactions with the stories on their feed. The metrics below gave us the confidence we needed that a list redesign would likely be effective. We discuss this process in more detail in our [list beats grid post](http://blog.getprismatic.com/list-beats-grid-linear-feeds-perform-two-to-three-times-better-than-grids-2/).




<table style="width:800px">
	<tr>
  		<th>Metric</th>
  		<th>Increase on List vs. Grid </th> 
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

**User Studies**

User studies provide insight into user behavior that metrics cannot - metrics tell us the “what”, and user studies tell us the “why”. Metrics can’t tell you how exactly to improve a design, only whether your design is better or worse than another. 

We conduct both qualitative and quantitative studies, selecting the method appropriate for the information we need. These include surveys, in-person interviews, remote usability studies, card sorting, etc. We’re always combining methods as we iterate on our design process. 

We recently conducted an in-person study showing people mocks of our feed redesign to understand whether people could easily distinguish the boundaries between stories. After the study, we found three story units in particular were prone to confusion. We generated a number of different variations that we thought could solve this issue. The number of variations and audience diversity we wanted to test with meant that in-person testing wasn’t feasible, so we used Mechanical Turk to run a remote test. Throughout this process we used both qualitative and quantitative studies to give us insight into user behavior. We describe this process in more in detail in our [Usability Testing for Detailed Design Using Mechanical Turk post](http://design.getprismatic.com/usability-testing-for-detailed-design-using-mechanical-turk/). 

**Implementation Considerations**

Implementation considerations are important inputs both in terms of opening up and constraining possibilities. This makes it important for design and engineering to work together in the design process from defining goals onward. Both design and engineering can be the source of new possibilities, or constraints due to technical complexity or the estimated cost-benefit of solving the problem in a certain way.

When we began exploring redesigning the grid view to be a list, we wanted to create a plan that allowed us to validate this direction as cheaply as possible before going all in on resdesigning a responsive list across all screen sizes. Design and engineering paired to see if it would be possible to hack up a list view on top of the existing grid view, by just merging the grid cells. 

So we took this:

![](/content/images/2014/May/preview-grid-1.jpg)

And hacked this together from the existing code:

![](/content/images/2014/May/preview-list-1.jpg)

Within a day, this small incremental list view redesign gave us sufficient evidence that investing in this path was a good move. This is a great example of how design and engineering working together in clever ways when taking implementation considerations into account.

**Assumptions**

Assumptions are important for the simplification and speed of the design process. Explicit assumptions encourage better reasoning and more informed debates. On the other hand, unstated assumptions often lead to unproductive arguments. Explicit assumptions are helpful when you lack information or are worried it is too time consuming or expensive to get it. In this case, assume your best guess, write it down, and move on. Either plan how you’ll merge in the information you need to get to validate or invalidate that assumption, or just see if the assumption will motivate someone who strongly doubts the assumption to help get the information you need.

When we redesigned the Prismatic home feed this year we assumed bigger images would generally be better. We didn’t have data to back this up outside of observing that many successful products seem to have better results with a bigger focus on images, and we’d heard a couple of anecdotal numbers from friends that work at some other major internet companies. We agreed to move on with bigger photos, because we can measure the impact of photo size on engagement later when we do a more systematic exploration of information density in the feed.

###Plan

After we’ve agreed on a good set of goals and gathered the inputs we need to inform decisions, we start planning the design. We explore the space of solutions and test a bunch of ideas as opposed to trying to design the single best solution. It helps us focus on decomposing the problem into smaller problems and make sure we’re always thinking about design solutions as hypotheses that we aim to test and learn from, rather than trying to design the ‘right solution.’

This focus ensures that we are thinking about how to separate the most difficult, uncertain, or valuable parts of a design problem that we can iterate on in isolation from other parts of the design that are not as important. Planning design is where we get into a lot of reasoning about the sequence of work and what ships when. We’ll normally kick off a design by working on the goals and inputs a bit, and then jump into the plan while we are still gathering many of the inputs. In this way, we’ll be able to send the goals, inputs and very early thinking to others for review while we’re still working on wires. The benefit is that we can get team alignment up front, rather than hear disagreement about goals or fundamental direction two weeks into a design when we’re reviewing polished pixels. We aim for the granularity of the reviews to match up with the granularity of design at the time of the review - so we’re reviewing goals at the beginning and pixels at the end.

Once we’re going from the early design goals and inputs to getting started, we go through a process of sketching/ideation based on what we know so far. Wireframing, visual structure and pixel details all begin while we’re gathering inputs and filling in all the reasoning for the design plan.  Throughout all of these steps, we’re continuously referencing the goals and inputs to make sure we’re reasoning clearly with good information and focusing on the right problems, both in filling in wires, structure, and pixels, and in terms of filling in our design plan.

**Group sketching**

We often kick off a new direction as a team with short sketching sprints to generate ideas as broadly as we can. We do a few 10–15 minute sketching sessions with rounds of discussion in between. The goal is generating as many ideas as we can, exploring divergent themes and covering as broad a territory as possible. We ignore constraints to ensure we’re going as broad as we can.  During the discussion between each sketching sprint, someone takes notes, including noting the best concepts in each round of discussion. Then, we discuss them all as a group and narrow down to a few options we want to wireframe.

![](/content/images/2014/May/sketches-1.jpg)

**Wireframes**

After we choose a few directions that best satisfy our goals, we start to explore them in wireframe fidelity for each option. We explore visual layout structure using mock text and images, and without meticulous pixel detail. We also use the goals and inputs to reason about which directions seem to be working better and spend more time iterating there.

We’re big fans of ‘exploration maps’ - infinite canvas artboards in Sketch. When we’re in this stage of exploration, we simply take the artboard we’re designing in and copy it to the right as we iterate, leaving a quick note about the reasoning behind each step. This way, we can see a clear line of reasoning as to why certain elements are styled a certain way or why they’re placed where they are.

![](/content/images/2014/May/exploration-map-1.jpg)


**Visual system**

After a round of wireframe critique with design and engineering, we start setting up the visual libraries we’ll need to explore our options at a detailed pixel level. We set up the grid, start setting up type size and scale and gather any design patterns we’ve previously designed that apply to the design we’re working on. We value reuse over redesign - we always try to use existing UI elements from other places in the product, not only for consistency, but for ease of implementation. As we’re narrowing in on the details we bring in front-end engineers to talk more in-depth about how to implement the design. 

![](/content/images/2014/May/grid-1.png)

**Pixel Detail**

We use Sketch for both wireframe explorations and pixel detail, which saves us some unnecessary translation time. During detailed design, we use the same approach of multiple artboards to document separate flows as we do during exploration.  For example, we’ll often evaluate a wide range of settings for type size, spacing, or color. All the same goals, inputs and reasoning apply here, because the concepts all have to manifest in pixels and stylistic changes can have a massive impact on the direct manifestation of the design goals.

We also practice what we call ‘universal design’; designing for all supported screen sizes at once. A separate Sketch file uses multiples artboards to show how a screen will change responsively for each supported screen size range.

![](/content/images/2014/May/universal-design-1.jpg)

**Conclusion**

Making great design decisions is difficult because you are marrying the creative process with the logical process of reasoning through the goals, inputs, and plan.

One of the greatest challenges is allowing both the creative and the logical processes to work together and play to the strengths of each. If the design team is excited, instincts can run wild ahead of defining goals or collecting inputs. This can be OK, but it’s important to be careful that the creative and logical parts of the design process work together in steps. If you take too many steps before evaluating against the goals, you can end up in a state where there are mistakes designed upon mistakes, and you have to throw away a lot of work. Often something appears to be a good decision when focusing on one goal, but then breaks down when looking at another goal.  It’s also easy to let logical data-driven decision making disrupt ideation. Sometimes precise goals and data doesn’t help you to ideate your way to a good starting point and the logical process of choosing needs to be fed by the creative process to generate designs to choose from. 

It’s important to go with the flow and allow flexibility in the design process, but at the same time ensure that we are avoiding the extremes of either allowing logic and data to block the creative process, or allowing the creative process to go too far without being grounded in logic and data. We’ve had a lot of success with the flexibility of the ‘Goals, Inputs, Plan’ model for making decisions, rather than adhering to a rigid step by step design processes. This approach gives us the flexibility to take different paths to problem solving, while at the same time still each following an agreed upon system for making decisions. This has resulted in consistently better decision making, fewer opinion-driven arguments, better design critiques, and more uninterrupted ‘flow state’ work. Objectively, we’re making the numbers go up and subjectively, the design team feels better about the work.
