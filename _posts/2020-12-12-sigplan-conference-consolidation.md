---
title: Should we consolidate SIGPLAN conferences?  If so, how?
tags:
   conferences
layout: default
comments: true
---


Should we consolidate SIGPLAN conferences?  If so, how?
=======================================================

*"Never let a good crisis go to waste."* - attributed to Winston Churchill

People leading SIGPLAN and its conferences have been asking what SIGPLAN should do post-COVID.  Do we go back to business as usual, with 4 major in-person events per year with scattered smaller events in the community?  Or should we take this interruption, and what we have learned about online conferences, to do something different?

Here are three challenges we face with our current conference structure, followed by a discussion of potential solutions and a concrete proposal.

Challenge #1: Climate
---------------------

We are all becoming more aware of the need to reduce carbon emissions, and flights of any kind--including international conference travel--add a lot of carbon to the atmosphere.  Some believe all international conferences should be moved online.  Others get a great deal of value out of in-person meetings, and see virtual conferences as a poor substitute.  Regardless of which camp you fall into, it seems that as a community, we should (and will) end up taking fewer trips in the future...and so it would be great if we can get more out of the ones we do take.  This suggests a strategy of moving some conferences online permanently, and consolidate in-person conferences as much as possible.


Challenge #2: Fragmentation
---------------------------

Compared to some communities such as HCI, Graphics, and to some extent Software Engineering, the PL community is fragmented into many conferences.  SIGPLAN sponsors 4 flagship conferences (the 3 that are in PACMPL, plus PLDI) and there are a number of colocated conferences as well as PL-related conferences sponsored by other organizations.  Since few people can afford the time and money to attend 4 (or more) conferences every year, it is hard to reconnect with all our colleagues, even though we travel more than we would like.  This is another reason to consider consolidation, particularly of whatever events remain in-person.


Challenge #3: Rankings threaten Intellectual Diversity
------------------------------------------------------

Historically, the variety of publication venues in Programming Languages has been a major benefit to the diversity of our field.  Our conferences have distinct personalities, and many serve as homes that nuture particular subcommunities.  While they overlap, each pays particular attention to certain problems and styles of scientific work.  Our field is stronger because POPL nurtures PL theory and connections with math; because PLDI nutures work on compilers and performance; because ICFP nutures work in functional programming; and because OOPSLA nutures research on OO as well as connections with industry and software engineering.  Beyond the SIGPLAN flagship conferences, this benefit is enhanced by more specialized venues like GPCE and DLS, two conferences colocated with OOPSLA at SPLASH.

Regrettably, conference ratings such as Google Scholar's [h5 indexes](https://scholar.google.com/citations?view_op=top_venues&hl=en&vq=eng), [CORE](https://www.core.edu.au/conference-portal), and [csrankings.org](http://csrankings.org/) have widened the perceived differences in quality among these venues.  I have heard departments urge faculty to submit primarily to top conferences to enhance the department's position on CSRankings, for example; this even affects ICFP and OOPSLA due CSRankings' arbitrary rule that there can't be 4 top conferences in a field.  As a consequence, submissions have been increasing at the top venues and decreasing everywhere else.  Maybe this is good with respect to the challenges mentioned above, as papers published in the top conferences get more visibility and we see each other there.  But it threatens the major mechanism our field uses to preserve diversity.

Many of our colleagues who publish work in subareas or research styles that differ from those favored by the conferences at the top of these rankings are very concerned about this trend.  Actually, we all should be concerned about this; diverse fields are stronger fields, and ideas that revolutionize an area often come from subareas that have been disfavored (think of deep learning, emerging out of disfavored neural network research, or program repair, which originally emerged out of genetic programming).


Challenge #4: Submission Calendar
---------------------------------
 
It's nice to have 4 deadlines around the year for major SIGPLAN conferences, because work matures at different times, and because timely resubmission becomes possible after fixing issues with a paper identified by reviewers.  If ratings-driven consolidation continues, though, this benefit is threatened.  

 
Potential Solutions
-------------------

From the discussion above, there seems to be a conflict: on the one hand, climate and fragmentation drive us towards consolidation, while intellectual diversity and the submission calendar suggest trying to preserve or even diversify our current conference offerings.

Fortunately, having different conferences are not the only way to solve the intellectual diversity problem.  While consolidating conference tends to reduce diversity if run according to the status quo, it is possible to organize a "big tent" conference in a way that deliberately preserves and could even enhance intellectual diversity compared to the status quo.

Consider our colleagues in HCI.  The CHI conference has 15 topical subcommittees, each with its own chairs and program subcommittee.  Authors select the subcommittee best suited to their work, and can list two if necessary.  There is no hierarchy among subcommittees; which subcommittee was selected is not obvious in the proceedings or program.  We could adopt a similar solution to ensure that papers in the wide variety of areas we publish retain good homes.  In practice, we would have more than 4 subcommittees, as we could intentionally design the areas rather than just take the ones that have naturally evolved into the current ICFP/OOPSLA/PLDI/POPL.  As an example, one such area might be metaprogramming and generative computing; another might be dynamic languages.

There are also known models for fixing the submission calendar issue: journal-first submission (e.g. as in TOPLAS submission, with later presentation in a SIGPLAN conference) and multiple deadlines (e.g. as the Programming conference/journal hybrid does).  For SIGPLAN, the TOPLAS option is already in place, and I would personally be pretty happy with that.  If there are those who aren't, a Programming-style solution, where committee members agree to be on a conference committee for a year, with 3 different submission windows, might be a good alternative.


Concrete Proposal: A SIGPLAN Conference
---------------------------------------

I propose that we create one international SIGPLAN conference, meeting in-person annually, with remote attendance supported to the extent possible--so those with fewer financial resources or who wish to avoid travel for climate reasons can still participate.  SIGPLAN would replace ICFP, OOPSLA, PLDI, and POPL, and it would host a wide variety of specialized workshops.  The first one could be held in June/July 2022 (benefiting from summer academic break in the northern hemisphere, while avoiding August holidays) by which time we can hope COVID would be well under control.  All other SIGPLAN-sponsored conferences would be held virtually, perhaps with narrow exceptions for highly local conferences.  SIGPLAN papers would be published in PACMPL, as ICFP/OOPSLA/POPL papers are today.  To preserve and even enhance intellectual diversity, the PC would have subcommittees covering communities represented across SIGPLAN; we would look to CHI for organizational inspiration.  SIGPLAN would accept submissions throughout the year, with a late fall deadline for papers expected to be presented in any given year's conference.  TOPLAS to SIGPLAN would be one option for submission throughout the year, but having a standing committee where members agree to review a fixed number of papers over a year would be a valuable addition to consider.  The new conference would target publishing at least as many papers annually as the four major SIGPLAN conferences currently do when combined, and maintaining an acceptance rate that remains selective yet, for inclusivity, is at or above the average of those four conferences.


Alternatives and Variations
---------------------------

What are the major alternatives and variations?

 * The status quo is the most likely alternative--but in the status quo, the first three challenges above will become more and more problematic. I think we can do better.
 
 * We could make all conferences virtual; that would best address the climate problem, but I don't believe our community is ready to give up in-person, international events.  Doing this alone also does not address the challenges of the Rankings-driven threat to intellectual diversity in our field.
 
 * A SIGPLAN multiconference without the reviewing reforms above would make the Rankings/Diversity and Submission Calendar problems worse.  The existing conferences could accept papers year-round and then present them all at once, solving the Submission Calendar problem...but that would result in a duplication of effort where the conferences overlap, and we would be losing the opportunity to do *better* on diversity of papers by having more sub-area committees with a more logical structure than the current 4 conferences.
 
 * Jens Palsberg has suggested adding an annual in-person conference, while retaining ICFP, OOPSLA, PLDI, and POPL as virtual.  I definitely think this is worth considering; it is a smaller change from the status quo.  But, it loses some of the benefits a broader change could bring, and there would be some redundancy between the in-person conference and the virtual conferences.
 
 * The proposal above could be implemented without in-person paper talks--the talks could be given virtually, as papers are accepted--but instead feature a variety of workshops, some of which might invite authors of recent papers to present.  This is a reasonable variation to consider.  It is arguably less egalitarian, in that not everyone who publishes a paper gets a speaking slot, but only those whose work overlaps with a workshop.  Hybrids are possible: perhaps most papers could be presented in topical workshops, with a "miscellaneous papers" track for the papers that don't fall into a workshop area.

 
Let's Work Together
-------------------

Please consider the challenges outlined above.  If you are convinced, upon reflection, that they are real, consider this proposal as a solution that, while undoubtedly imperfect, could improve on the status quo in multiple dimensions.  If you'd like to work together on fine-tuning, fleshing out, and implemeting this proposal, let me know, and we can work with SIGPLAN leadership to convene a group to work on it.

*My thanks to colleagues who provided feedback on earlier versions of this post.*