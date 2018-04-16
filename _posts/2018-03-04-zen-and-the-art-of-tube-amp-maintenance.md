---
layout: post
title: "Zen and the Art of Tube Amp Maintenance"
description: ""
category: 
tags: [tube amps,hardware,rdd]
---
{% include JB/setup %}

Here are some notes from a lightning talk I gave at North West Ruby Group.

*Abstract:* 

Production hardware has a high cost of change, while testing can be difficult in tightly-coupled circuits, because they tend to be on production PCBs.

In software, we are lucky in that our productionised version can also be the same code base as our development version.

In audio hardware development, it's common to have a development board, or breadboard, and then a production version on a PCB. Testing one does not necessarily translate to the other.

In a circuit on a breadboard, we have passive and active components that we can debug:

- On their own
- As part of a block of functionality (e.g. a high-pass filter)
- As part of the entire circuit

This is achieved by taking components out, passing signal through a particular block of functionality, or by extracting pieces onto a separate breadboard.

We should strive for this testability when writing programs.

Essentially by breaking down our code as if it was a series of resistors, capacitors and opamps, we realise that it is almost always possible to have our programs in a breadboard-like state, while having them production-ready.

## Documentation

First of all, it's worth pointing out that when we acquire knowledge about how our system does behave, it's worth making a note of this. Knowing how things will behave and when our expectations are no longer met is very useful information. However, documentation is static and often rots - direct feedback is more useful in both the short and long term.

## Informal Reasoning

Informal reasoning is being able to logically step through the behaviour of our systems. We do it all the time when designing both hardware and software, and it's our first port of call in most situations. We often talk about this as 'having the program in our head.' The simpler the modelling approach we've taken to our domain, the more likely that we can reason about it effectively in this way.

It's often informal reasoning that leads us to the doorstep of the design we end up using, or to the general neighbourhood of the bug we're trying to track down. The more we work in an area, generally the better our ability to reason about a system becomes. Combined with a general intuition born of experience, it's a powerful and essential tool.

That said, when it comes to the nuts and bolts of testing a feature and getting it ready for production, or debugging a piece of code and verifying that it now works, informal reasoning isn't enough. We need empirical tests.

## Feedback

So I'm a big fan of Bret Victor and [Inventing on Principle](https://vimeo.com/36579366). One thing that strikes me is that we're lucky in programming that we have so many different ways of getting feedback, whether it's Guard-style running of specs or the RDD approach of the Clojure world.

It seems to me that TDD and the spec runner pattern makes explicit that the goal is reasoning about the system, and feedback, as much as it is about stability of the system.

When you're in an environment where this instant feedback is absolutely not an option - for example building a tube amp or a pedal, where you only know the results of your work when you are at the finish line, and whether or not your circuit works - then you realise how lucky we are as programmers.

## Debugging in circuit versus in isolation

The simplest circuits are often made on breadboards, which allows for two desirable properties:

- Easy change
- Easy debugging

The circuit can be tested holistically, end-to-end, and by taking out individual elements 

However, in order to stabilise and productionise our circuit, we have to move beyond breadboards - you can't ship a product in a form that components might fall out at random!

## Turret boards and PCBs

After breadboarding - although that step is optional - simple circuits are often constructed on stripboard, or in the case of amplifiers, turret boards. Turret boards can be designed with the known shape of the circuit in mind, but they are also sometimes just two rows of turrets facing one another - in that sense, similar to perfboard or stripboard.

This is great as it's a blank canvas to build on top of, but bad because it's not very repeatable.

Think about it like a small code spike that wasn't written with testing in mind - how do you make it repeatable? How do you stabilize your design thinking? In the case of electronics, we make a PCB.

## Our tightly-coupled systems

When we make a PCB, we reify our circuit design - which is expressible as an abstract schematic, just as our programs are abstract syntax - into a concrete form, with traces, holes for components and a clear control flow for signal.

Signal is kind of analogous to control flow in our programs, and both the signal line and power line are often points that need debugging.

Where we don't have feedback, we don't tend to notice a tight coupling emerging. Now, partly this is a critique of my PCB design, but many professional manufacturers will split out PCBs into blocks that can then be debugged.

The Ampmaker N5X amp that I built even partly has this quality - the power board and the main circuit can be indepedently debugged. In fact this was necessary when I indavertently grounded a part of the main board and caused a small electrical fire.

However, we should be keen to apply this lesson up-front. Assuming our system goes wrong, how can we debug it? What debug tools are available to us as creators?

## A better way

Since inherently software is easier to change and - in development at least - has fewer consequences on the whole than hardware development, it is desirable to learn lessons from hardware and apply them. Coupling and feedback are the two things that we've discussed here.

### Handling coupling

In an OO language:

- Pragmatic handling of state
- Class methods
- Primitive data structures

Or, you could just use a data-oriented language... but this isn't a sales pitch.

The goal is to separate out pieces of your program so that you can verify parts of it without having to scaffhold the rest.

The granularity of this could be at function level or object level, but thinking at the level of a function - even in an OO language - not only makes for easier unit testing, but easier informal reasoning.

Making state explicit, and the interfaces between things (e.g. object instances) more explicit, can help with this.

Using class methods and passing around data structures as opposed to using side-effects on instances is a way of managing the complexity of interactions between components in your systems.

Whether this piece of functionality is an operation on an object, or if it has been placed here for a code organisation reason is a good rule-of-thumb for class and instance methods.

### Breadboard-driven development

IRB or similar debuggers can be used to drive your development. When combined with moving from implicit side-effects in OO code to methods that pass around data structures, your code becomes more testable in both the exploratory, poking sense, and the automated testing sense.

One of the things I noticed most about how my Ruby changed after using the repl in Clojure development was leaning on IRB a lot more when building features in Ruby/Rails, as opposed to taking quite a dogmatic TDD approach. If you have well-decoupled pieces of logic, you can extract code run via IRB and reify those as tests.

If we think about our code like a series of resistors, capacitors and opamps, the breadboard model, where it is easier to debug the component, part of circuit, and circuit as a whole is basically the goal of writing good programs.

RDD, or repl-driven development, is a common name for this.
