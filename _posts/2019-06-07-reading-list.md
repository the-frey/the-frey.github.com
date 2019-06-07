---
layout: post
title: "A reading list for software engineers"
description: "A mega list of all the advice and articles I think are really worth investigating"
category:
tags: [reading,hackers,programming]
---
{% include JB/setup %}

NB: Got suggestions? [Comment on the Gist I use to keep this updated!](https://gist.github.com/the-frey/baf8555ef01e107eb8b3cc2fbfc7d40d)

You should know in advance that I have a bias towards hacker and maker culture as having shared roots in creativity and creative thinking; that I ~like~ love functional programming, and I think that somehow (at least in the UK) in the software engineering game we manage to simultaneously do down both the importance of strong tech skills and of strong people skills (sigh). Both of these skills can be learned, as can, well, pretty much anything if you're a true hacker. Anyway.

# The most important stuff

This is the basics for working in a professional context. Your side hacks are largely a different game entirely.

- In terms of bang for buck, [Apprenticeship Patterns by Adewale Oshineye and Dave Hoover](https://www.oreilly.com/library/view/apprenticeship-patterns/9780596806842/ch01.html) is the book I've gone back to the most over the years. Crucially, if you don't want to splash the cash, it's available for free at that link.
- 'Agile' is largely a cult for overpriced consultants, but you should read [the original agile manifesto](https://agilemanifesto.org/)
- _Lean UX_, by Jeff Gothelf and Josh Seiden
- _The Design of Everyday Things_, by Don Norman
- _Drive_, by Daniel Pink
- _Accelerate_, by  Nicole Forsgren, Jez Humble and Gene Kim (although I _hate_ the jargon term 'devops' with a passion. Devops is when devs do ops. Simple...)
- Don't fall into the trap of [the expert beginner](https://daedtech.com/how-developers-stop-learning-rise-of-the-expert-beginner/) - always be in search of mastery.

> Absorb what is useful. Reject what is useless. Add what is essentially your own.

  _Bruce Lee_

Finally, decision making is hard. To be a doer, you have to get good at making decisions and owning the consequences. My friend Jake used to say "you've gotta pick a path!" whenever there was a quandry, and most of the time _any_ decision is likely to pan out better than not making one at all.

Or, as Rush had it, because they enjoy contradicting me, _"if you choose not to decide, you still have made a choice"_...

Getting good at owning mistakes and dealing with failure will make this easier to commit to over time, and you'll usually find you learn more from the failures than from the successes.

## Engineering skills and career advice

Don't focus on any one thing - learn general principles you can apply everywhere.

Learning _how to learn_ is the most valuable skill you can optimise.

Learn about:

- working with data, serialisation and deserialisation, contracts and consistency
- how things compose
- graphs, they appear everywhere and the real world isn't tabular!
- user research: talking to users is a super power that will help you build the right thing
- thinking about whole systems
- breaking large problems down into small ones

I could put zillions of recommendations here, but I'll mainly recommend a few videos (in this order):

1. [Inventing on Principle - Bret Victor](https://vimeo.com/36579366)
2. [Simple Made Easy - Rich Hickey](https://www.infoq.com/presentations/Simple-Made-Easy)
3. [Web Development is Distributed Systems Programming - Mikaela Patella](https://youtu.be/KxMK2AklpNY)
4. [Turning the database inside out with Apache Samza - Martin Kleppmann](https://youtu.be/fU9hR3kiOK0)
5. [Hammock Driven Development - Rich Hickey](https://youtu.be/f84n5oFoZBc)
6. [Death to Bullshit - Brad Frost](https://vimeo.com/63437853)

Read:

> Whenever I’ve had management responsibility, I’ve subconsciously put people into two buckets.  Problem solvers take a problem I have and make it go away.  Problem transformers take a problem I have and solve it by bringing me the next problem.  (I’m omitting non-performers who don’t solve problems at all as beyond the scope of this post.)

  _Erik Dietrich_, from _The Polyglot's Dilemma_.

- [The Polyglot's Dilemma - Erik Dietrich](https://daedtech.com/the-polyglots-dilemma/)
- Martin Kleppmann's Book, _Designing Data Intensive Applications_.
- [Steve Yegge's Platform Rant](https://gist.github.com/chitchcock/1281611)
- [Great Hackers - Paul Graham](http://www.paulgraham.com/gh.html), although it's worth saying that I think what he says about political incorrectness is utter shit. Being kind and showing empathy have nothing at all to do with whether or not you can build cool things in a cool way.
- [Always Be Leaving](https://daedtech.com/always-be-leaving/) was a post that really blew my head off, as I'd been doing it for years without even realising it. Related to Patrick McKenzie's 'increase revenue, or decrease costs' north star, Erik Deitrich says you should have goals going into every job, as well as success and exit criteria; in other words, treat every job like the semi-temporary arrangement it really is, whether you're permanent staff or a contractor

### Life at startups and small companies vs big ones

Even before I got into engineering, I'd mainly worked for small companies, although I have also worked for national retailers and for the NHS.

I could write about this one forever.

- [PG's article](http://www.paulgraham.com/really.html), which also rings true for most creative side projects, bands, communities and what not that I've got involved in over the years

### Programming languages

I might have to expand on this in future, as I don't really know where to start (or end!)

- [The Bipolar Lisp Programmer](http://marktarver.com/bipolar.html) is one of the best non-macho, non-heroic narratives of what the hacker lifestyle and ethos can entail. As its author says, "The final conclusion [is], that Lisp like life, is what you make of it."

### Theory

Again, so much to post here, so I will update as things spring to mind.

- Bartosz Milewski's [Category Theory for Programmers blogpost series](https://bartoszmilewski.com/2014/10/28/category-theory-for-programmers-the-preface/), also [available as a PDF and book](https://github.com/hmemcpy/milewski-ctfp-pdf).

## Management

I don't think I've managed people long-term enough to really write about it myself, but luckily there are plenty of people that have.

Most of it boils down to:

- your reports are your number one priority (and yes, I mean above everything else, bar maybe the software being down in prod)
- never reschedule 1-2-1s
- trust your reports, and fight their corner
- seek to help your reports become more confident
- seek to help your reports become more creatively fulfilled
- (this one is hard) try and talk to them in the way they want to be talked to, according to their personalities, not how you like to talk or be talked to. This also applies for motivation.

Read:

- [The Update, The Vent, and The Disaster](http://randsinrepose.com/archives/the-update-the-vent-and-the-disaster/) has proven continually useful as a field guide.
- Why are people on our teams burning out? [Here's why](https://www.forbes.com/sites/johnrampton/2015/05/13/the-6-causes-of-professional-burnout-and-how-to-avoid-them/#2584e2fe1dde).
- Why the idea of a 'meritocracy' is [often a fallacy](https://geekfeminism.wikia.org/wiki/Meritocracy)

## Hiring

I've only ever participated in the hiring and interview process for a large organisation, and maybe I'll blog about that some time. Until then, read Joel Spolsky and PG:

- [The Guerrilla Guide to Interviewing (version 3.0)](https://www.joelonsoftware.com/2006/10/25/the-guerrilla-guide-to-interviewing-version-30/)
- [The Python Paradox](http://www.paulgraham.com/pypar.html), aka 'Why You Might Want To Use Clojure Or Haskell When You Start A Company'

Yeah, so smart people that get things done, and no assholes. Sounds simple, right?

A few big things I learned, however, I will write about here:

- 'Are we excited to work with this candidate?' is a good gut feel litmus
- Pay people flat bands, so everybody at the same level is paid the same, regardless of negotiating ability
- It's much more costly to hire the wrong person
- If you're not sure, the answer is a 'no'
- Every person you hire should raise the standard across the board in some way
- Every person you hire ideally should bring some new experience or perspective to the team (diversity works [in my and others' experience](https://hbr.org/2016/11/why-diverse-teams-are-smarter), y'all, and [maybe even in cash terms](https://www.forbes.com/sites/annapowers/2018/06/27/a-study-finds-that-diverse-companies-produce-19-more-revenue/#2464eabd506f))
- You should probably [check your job ad to see if it's masculine or feminine coded](http://gender-decoder.katmatfield.com/). There are free tools (like the one linked) online.

## Personal and professional development

Consider the value apex when thinking about your job - is it a fair trade in terms of knowledge, or is it all take, no give on their side?

> The reason that skilled employees quit, however, is a bit more complicated. In virtually every job, there is a peak in the overall value (the ratio of productivity to cost) that an employee brings to [their] company. I call this the Value Apex.

> On the first minute of the first day, an employee’s value is effectively zero. As that employee becomes acquainted with [their] new environment and begins to apply [their] skills and past experiences, [their] value quickly grows. This growth continues exponentially while the employee masters the business domain and shares [their] ideas with coworkers and management.

> However, once an employee shares all of [their] external knowledge, learns all that there is to know about the business, and applies all of [their] past experiences, the growth stops. That employee, in that particular job, has become all that [they] can be. [They have] reached the value apex.

> If that employee continues to work in the same job, [their] value will start to decline. What was once “fresh new ideas that we can’t implement today” become “the same old boring suggestions that we’re never going to do”. Prior solutions to similar problems are greeted with “yeah, we worked on that project, too” or simply dismissed as “that was five years ago, and we’ve all heard the story.” This leads towards a loss of self actualization which ends up chipping away at motivation.

> Skilled developers understand this. Crossing the value apex often triggers an innate “probably time for me to move on” feeling and, after a while, leads towards inevitable resentment and an overall dislike of the job. Nothing – not even a team of on-site masseuses – can assuage this loss.

[Up or Out](http://thedailywtf.com/articles/Up-or-Out-Solving-the-IT-Turnover-Crisis)

- Are you holding things together, instead of programming? Are you doing key but uncredited work? If so, you might be a 'glue' person. Tanya Reilly's [Being Glue](https://noidea.dog/glue) talk is the wake-up call you need.
- Are you spending time solving other peoples' problems? If you're not getting the credit, then don't do it! [Other People's Problems is a great article](https://medium.com/@skamille/opp-other-peoples-problems-d7eb174724ee) by Camille Fournier that even has a helpful flowchart.
- [Don't Call Yourself A Programmer, And Other Career Advice](https://www.kalzumeus.com/2011/10/28/dont-call-yourself-a-programmer/) by Patrick McKenzie is in my 'harsh but fair' category. Repeat after me - increase revenue, or decrease costs, that's the name of the game.

If your current job isn't working for you then continue to...

### Getting jobs, changing jobs

Here's a non-exhaustive list of questions I will try and ask a prospective employer:

- do they talk to users?
- what’s the tech stack?
- do they understand their problem area adequately?
- is their product a good market fit?
- what is the quality of the people that work there?
- how do they handle and respond to change?
- how long is their runway? (if applicable)
- do they pair? (red flag if never, red flag if always. Pairing for critical bugs seems smart to me, ymmv)
- can I use the tools I prefer?
- how much autonomy do individuals and teams have?
- do they allow remote? (lower priority if they’re local to me)

If you get an offer, make sure you negotiate politely but firmly to get to a mutually beneficial arrangement.

Read:

- You might know your technical strengths, but knowing how other people see you is incredibly valuable, and will help you both in positive personal growth ways, as well as in the dark arts of knowing how to influence others to see things your way. [Anna Shipman's way of gathering feedback when leaving a job is brilliant](https://www.annashipman.co.uk/jfdi/johari-window.html).
- [Steve Yegge's rant about getting a job at Google is always an entertaining and useful read](https://steve-yegge.blogspot.com/2008/03/get-that-job-at-google.html).
- Anna Shipman also wrote a great post on choosing your career path once you've been in the game for a while, and how you find your 'perfect' job; [check it out here](https://www.annashipman.co.uk/jfdi/finding-next-level-job.html).
