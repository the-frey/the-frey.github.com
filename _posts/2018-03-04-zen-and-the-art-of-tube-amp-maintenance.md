---
layout: post
title: "Zen and the Art of Tube Amp Maintenance"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Work in progress - this is a lightning talk I'm giving, and I'll turn the slides into an article ASAP.

*Abstract:* 

## Documentation

## Informal Reasoning

## Feedback

So I'm a big fan of Bret Victor and [Inventing on Principle](https://vimeo.com/36579366). One thing that strikes me is that we're lucky in programming that we have so many different ways of getting feedback, whether it's Guard-style running of specs or the RDD approach of the Clojure world.

It seems to me that TDD and the spec runner pattern makes explicit that the goal is reasoning about the system, and feedback, as much as it is about stability of the system.

When you're in an environment where this instant feedback is absolutely not an option - for example building a tube amp or a pedal, where you only know the results of your work when you are at the finish line, and whether or not your circuit works - then you realise how lucky we are as programmers.

## Debugging in circuit versus in isolation

## Our tightly-coupled systems

Where we don't have feedback, we don't tend to notice a tight coupling emerging. Now, partly this is a critique of my PCB design, but many professional manufacturers will split out PCBs into blocks that can then be debugged.

The Ampmaker N5X amp that I built even partly has this quality - the power board and the main circuit can be indepedently debugged. In fact this was necessary when I indavertently grounded a part of the main board and caused a small electrical fire.

However, we should be keen to apply this lesson up-front. Assuming our system goes wrong, how can we debug it? What debug tools are available to us as creators?

## A better way

