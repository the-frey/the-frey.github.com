---
layout: post
title: "First Use of StackOverflow"
description: "Stack Overflow is a true example of web greatness, here's a bit about my experience with it so far"
category: 
tags: [stackoverflow, help, rails]
---
{% include JB/setup %}

Okay, so obviously I've browsed around and searched through topics before, but this was my first time posing a question to the community. 

The problem was a number of local gemfiles were intermittently disappearing from my rails install, messing with everything from creating scaffolds to booting the rails server. I'd tried a few things with no luck, so I thought I would pose my first question. 

Within an hour I had a response - install the bundler gem. gem list revealed that actually it was already installed, but this would lead my mental exploration to the point of solution. 

A couple more responses came in, and specifying that I was a relative newbie probably resulted in things being pitched a little low - dependency problems due to wrong install, etc., until I realised when trying to boot the server today that if I had different versions of Ruby on my machine with different gems, that would account for rails suddenly being unable to find them. 

A quick search later and it appeared that I'd installed Ruby 1.9.2 to play with at some point and promptly forgotten all about it. That deleted and the rvm default properly set and we're back in business. Naturally I feel like an idiot, but the key here is that I went back to the SO thread and posted my solution to the problem. A quick search of Google reveals this is one that occasionally comes up and is kind of annoying to fix (as you may not remember installing an old version of Ruby/misconfiguring rvm); moreover, if you're a bit more experienced I guess it's embarrasing enough not to warrant a follow-up post humbly revealing the solution. 

Either way, no such concerns here, and it certainly goes a long way towards that old saying about laying out your tools before starting work. I'm also very pleasantly impressed by the StackOverflow response to a relatively niche problem posed by a new user - two responses were enough to kick my brain into gear and solve it, so that's a success in my book. 
