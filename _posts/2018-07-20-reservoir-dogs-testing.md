---
layout: post
title: "Reservoir Dogs Testing"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Primarily because I'm a massive contrarian, I've been skeptical of the cult of TDD since I got out from under it myself as a junior way back when. Now, obviously I do actually write tests, it's just my approach varies depending on language, project and context. Hell, in a Rails code base, I'd generally take a TDD approach purely because I can't reason at all about what it's going to do without writing something first. 

ANYWAY. I read [this amusing blog post](http://devcycle.co.uk/TDD-Thats-Design-Done/) and decided to find and replace every instance of 'tip' in the opening of RD to 'test'. There are some great bits...

> I don't test because society says I gotta. I test when somebody deserves a test. When somebody really puts forth an effort, they deserve a little something extra. But this testing automatically, that shit's for the birds.  As far as I'm concerned, they're just doin' their job.

 

<script src="https://gist.github.com/the-frey/5cce0f631ff28ec39a7637bff36d3ab6.js"></script>

As an aside, while grepping through the script, I noticed that my saying:

> The words "not possible" shouldn't be in an engineer's vocabulary.

Is ripped off from the opening scene as well. Plus, I'm a hypocrite, because I say no all the time. Then again, just because something's possible doesn't mean you _should_ do it. After all:

> Your scientists were so preoccupied with whether or not they could, they didn’t stop to think if they should.

I wonder sometimes if I could even communicate without movie quotes.
