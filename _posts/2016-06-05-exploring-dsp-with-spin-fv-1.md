---
layout: post
title: "Exploring DSP with Spin FV 1"
description: "Time to try something new..."
category: 
tags: [dsp,music,spin]
---
{% include JB/setup %}

I've been interested in the intersection between music and code for some years now, completing a Coursera course on programming for musicians (using ChucK) and then going on to experiment at some length with live-coding environments like Overtone and Sonic Pi (about which I've done a couple of user-group talks and hands-on sessions since)... but somehow it's never quite clicked as something that can keep my attention long-term.

Now isn't the time for my critique of coding for musical purposes or live-coding in general, but I suppose if nothing else it has prepared me to get interested in DSP. Having taken something of an interest in guitar effects recently it quickly struck me just how much overlap there is in common guitar circuits; painting with a broad brush (this is a big oversimplification), most analogue (or even, non-DSP) effects broadly fall into the family tree of amplifier (e.g. fuzz, overdrive, distortion) or delay (e.g. chorus, flanger, delay, reverb - though admittedly, these effects are often achieved in subtly different ways).

Either way, the upshot of this is that it quickly became clear that a deeper dive into modern digital processing in guitar effects would be the most interesting medium-term project to try and come up with something that would prove useful, and hopefully, somewhat original.

So, armed with that ambition I've been mucking around with the Spin FV-1, the chip that supposedly powers a lot of EQD boxes and other boutique gear like the Red Panda Particle. As a long-time fan of weird units like the Digitech Space Station, I figured this would be a fruitful avenue of exploration.

The first thing that makes the chip easier to dive into is the dev board - if you're familiar with an arduino then it shouldn't look scary. From there it's a relatively short process to get up and running, and there's lots of code examples (and helpful people) on the Spin forums to help you if you get stuck. However, if you're on a Mac, the official Spin ASM software doesn't work, so you have to either use a VM, use Windows, or switch between two machines. What a pain.

Luckily, for quick prototyping there's an ace tool from the community - Digital Larry's SpinCAD Designer, which is basically a graphical editing tool for messing with a number of pre-canned signal blocks. After about two hours of tinkering I'd come up with and exported a number of scratch ideas, and after a little bit of tweaking, two of those might well make their way into the finished _Atom Smasher_ pedal. We will see. 

For the more complex stuff, you still have to hand code however - and here's where I'm glad I've done live coding et cetera before. Even with Spin's flavour of assembly being a little more user friendly, it's still a slow iteration process when you're time poor, and developing dynamic delays that you intend on controlling with an expression pedal is not exactly easy when you've yet to build a test board around the chip that includes an expression pedal input. Never mind.

So yeah, exciting times all round. If you're into that sort of thing, definitely check out the FV1 and SpinCAD... though if, like me, you've got a _very_ specific sound planned that you want to create, you're going to eventually have to get your hands dirty and write some assembly. Ouch.