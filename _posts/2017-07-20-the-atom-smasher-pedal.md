---
layout: post
title: "Designing The Atom Smasher Pedal"
description: "I've spent a while making a digital delay pedal, and I thought I'd share some more details..."
category: 
tags: [pedals,code,electronics]
---
{% include JB/setup %}

<blockquote class="instagram-media" data-instgrm-version="7" style=" background:#FFF; border:0; border-radius:3px; box-shadow:0 0 1px 0 rgba(0,0,0,0.5),0 1px 10px 0 rgba(0,0,0,0.15); margin: 1px; max-width:658px; padding:0; width:99.375%; width:-webkit-calc(100% - 2px); width:calc(100% - 2px);"><div style="padding:8px;"> <div style=" background:#F8F8F8; line-height:0; margin-top:40px; padding:50.0% 0; text-align:center; width:100%;"> <div style=" background:url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACwAAAAsCAMAAAApWqozAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAMUExURczMzPf399fX1+bm5mzY9AMAAADiSURBVDjLvZXbEsMgCES5/P8/t9FuRVCRmU73JWlzosgSIIZURCjo/ad+EQJJB4Hv8BFt+IDpQoCx1wjOSBFhh2XssxEIYn3ulI/6MNReE07UIWJEv8UEOWDS88LY97kqyTliJKKtuYBbruAyVh5wOHiXmpi5we58Ek028czwyuQdLKPG1Bkb4NnM+VeAnfHqn1k4+GPT6uGQcvu2h2OVuIf/gWUFyy8OWEpdyZSa3aVCqpVoVvzZZ2VTnn2wU8qzVjDDetO90GSy9mVLqtgYSy231MxrY6I2gGqjrTY0L8fxCxfCBbhWrsYYAAAAAElFTkSuQmCC); display:block; height:44px; margin:0 auto -44px; position:relative; top:-22px; width:44px;"></div></div><p style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; line-height:17px; margin-bottom:0; margin-top:8px; overflow:hidden; padding:8px 0 7px; text-align:center; text-overflow:ellipsis; white-space:nowrap;"><a href="https://www.instagram.com/p/BV5fm7blsmN/" style=" color:#c9c8cd; font-family:Arial,sans-serif; font-size:14px; font-style:normal; font-weight:normal; line-height:17px; text-decoration:none;" target="_blank">A post shared by SSD Devices (@ssddevices)</a> on <time style=" font-family:Arial,sans-serif; font-size:14px; line-height:17px;" datetime="2017-06-28T21:28:26+00:00">Jun 28, 2017 at 2:28pm PDT</time></p></div></blockquote>
<script async defer src="//platform.instagram.com/en_US/embeds.js"></script>

## TL;DR

Okay, so let's start off with a few bits of info to set the scene.

- In May 2016 I visited my best man Jake in SF. After a conversation we had, I decided to get into digital effects as a hobby/side-project.
- In December 2016 I sold my first run of fuzzes, the EVA fuzz.
- In March 2017 I released the Transmission Killer, a distortion I designed.
- In March 2017 I finished the Event Horizon, a PT2399 and Belton-brick based delay/reverb I hacked together.
- In April 2017 I showed the first prototype of the Atom Smasher, the digital delay I'd been working towards for a year.
- By late June 2017 I had an order book that was 6 months to a year long, and worth more than 10k in revenue.
- I ultimately realised that the boutique pedal game is crazy competitive and tough, and I'm going to keep this a hobby.

So. That's the path.

## Why do it?

Really? Pedals are cool. I've spent my whole adult life playing guitar and obsessing over effects and it's only natural to want to both know how they work and contribute my own tiny little ripple to the great Gear Ocean. One of my many gigs is writing for Total Guitar (and Music Radar online) and this has had a nice synergy with that too - I can now attack a lot of gear with a screwdriver and get a pretty good idea of what's going on inside. The pedals has definitely led to a lot more work in that area, and since that's basically my dream job since I was about thirteen years old, that's already a pretty sweet outcome.

The real catalyst though was when on a whim I started a video blog/YouTube channel where I started demoing a bunch of the pedals I'd collected over the years. Although it hardly exploded in popularity, I soon realised I wasn't mega interested in just demoing other peoples' stuff, but was rather more interested in making it myself.

## How much work was it?

Probably comparable to writing a university dissertation a couple of times over. I'd estimate that between May 2016 and June 2017 I was putting in between 5 and 20 hours a week in on the pedals, depending on how much writing/band/label stuff I had to do that week around the day job. I haven't slept much.

That said, if I just had a day job and no other responsibilities, plus maybe a few more weekends, I reckon you could get there quicker than I did.

## How do you go about making a digital delay?

This is kind of a big question, so I'm going to split it down into a couple of bits about the process and chip I used, the [Spin FV1](http://www.spinsemi.com/products.html). I'll add some resources that might help you work out how to do it yourself as well.

### Hardware

Okay, so the FV1 is an SMD device which means that it's a bastard to solder. If you're inexperienced then maybe you want to design some other pedals first. I'd made _a lot_ of pedals and sold a run of fuzzes before I got to this point and even I fucked up the FV1 soldering on the first try. Luckily it's relatively forgiving, and as long as you're gentle and keep calm you won't damage the chip by reflowing the connections.

The best thread on this pedal is probably [this implementation](http://www.diystompboxes.com/smfforum/index.php?topic=104291.0) by Ice9 on DIYSB. The whole thread is packed with gold and loads and loads of good advice on how to make a pedal using the chip. Add to that the [FV1 Datasheet](http://www.spinsemi.com/products.html) on this page and you'll start to see how it all comes together. There's also loads of advice on the [Spin Forums](http://www.spinsemi.com/forum/index.php) on hardware implementation and how to for example use a three-way toggle to switch programs.

### Code

The FV1 uses a dialect of Assembly called SpinASM. You can write it in a text editor and compile it using a program that SpinSemi provide (more on that later). However, if you're looking to create more standard effects (chorus, phaser, reverb) for example, you might find that Digital Larry's brilliant [SpinCAD](http://holycityaudio.com/spincad-designer-2/) has what you need built in. In fact, I'd hazard a guess and say there's probably a fair few commercial boutique pedals whose code was generated by SpinCAD rather than hand-written. For mucking around with reverb blocks and pre- and post-processing, I've found it to be lots of fun and the glitch and pitch reverbs in the Atom Smasher were made using it. It's cool.

If you want to scale the heights of writing your own code, then there's lots of examples and help to be had over on the Spin Forums and there's some ace guides in the FV1 manual, as well as some useful supporting materials like [this primer on LFOs in the FV1](http://www.spinsemi.com/Products/appnotes/spn1001/AN-0001.pdf).

### Workflow

To put code on the chip, and bridge between the two worlds, you need to download the [SpinAsm assembly program](http://www.spinsemi.com/products.html) - it's on that page under the devboard 'software' heading. It will only run on Windows, so if you're on a Mac you're going to have to borrow somebody's PC. I write my code in Sublime Text on the Mac (or use SpinCAD, depending on what I'm doing), then put that on my wife's Windows laptop and paste the code into the SpinASM program and build it from there.

Some projects like the DIYSB project from Ice9 allow you to flash the chip in place in the actual pedal, but I use the Spin Dev board as it can be connected easily to the computer by USB and for me, is worth the money to reduce the hassle of mucking around with the board in the pedal. The PCBs I designed allow for flashing on the board (in fact, using Ice9's method in the same way) but in reality I've never found myself going that route.

## What I've learned

Whoa. Where do you start? I'll go with a few obvious ones though, besides the 'learned electronics' and 'learned assembly' which should be self-evident.

- Measure twice, cut once. Seriously, double-check everything on the hardware side. You _will_ have made mistakes. Every time. In fact, when you fix a mistake, you'll probably have created a new one. There's a reason people make clones, and it's because as soon as you deviate from an established schematic, things start to go wrong.
- Debugging hardware is often harder (to me, at least) than software. It's less forgiving of mistakes or screwed-up tolerances, and the iteration process is slow.
- ALWAYS INCLUDE NOISE FILTERING ON YOUR POWER LINE.
- Once you get a physical product into the wild, things will go wrong that you didn't expect, and again, it's sometimes hard to understand what, let alone why something has gone wrong.
- Prioritizing cost of time versus cost of materials is crucial, both during the prototyping phase and during any build run. As mentioned above, this is doubly tricky as your north star should be 'measure twice, cut once.' An example of this would be designing PCBs that are easier to solder in terms of part placement and how tight it is versus cost - you're likely to find fewer mistakes and less stress, and that will be worth the extra cost (so long as you don't, say, introduce noise into the circuit).
- ALWAYS INCLUDE NOISE FILTERING. SERIOUSLY GUYS.
- Like with programming, smart people will have figured out most of the problems you're going to encounter. Get good at researching the path other people have taken and reading abstract material related to the problems you're working on if you can't find directly related material. If you get stuck, then don't be afraid to ask - two heads are better than one, and we all make mistakes, so don't feel like because somebody points out that you've done something dumb that makes your work invalid. Nobody has ever succeeded in a vacuum, so don't fall into the trap of pride when you're down or hubris when you're doing well.
- Don't take the piss and try your best. Be honest with customers and polite to people that help you on the way. Give credit where credit is due and thank people that are nice about your stuff or post about it.
- Projects/new pedals are very time-consuming. Build for what you want to hear, not because everybody and his uncle has a full pedal range that you feel like you need to emulate. I like tremolo but have zero interest in making one, so... I haven't made one. This will allow you to focus on the projects that matter to you.
- Accept that sometimes you will try and not succeed. Know that you will reach your destination in the end anyway.
- Making money out of pedals is hard. As soon as you've sold a few, you go "wow, maybe I can quit my job!" Then you add up the costs, likely failure rate, amount of time taken to make each pedal, cost of marketing, up front parts cost, et cetera and realise that this is a tough, tough game. That's why I'm happy to stay a hobbyist.

## Conclusion

So obviously if I was still 21 years old, this would be a life-changing thing; for now it's just an interesting project that I'm glad I've undertaken and I'm glad has given some people some joy. Hopefully as I ship these, they will bring happiness to a few more people.

Maybe at some point I will release the schematics and files for this project, I don't know. Haven't really decided where this is going yet, but at this point it's somewhat outgrown a hobby side-project.

## Links

- [DIYSB FV1 Thread](http://www.diystompboxes.com/smfforum/index.php?topic=104291.0)
- [Spin FV1](http://www.spinsemi.com/products.html)
- [Spin Forums](http://www.spinsemi.com/forum/index.php)
- [LFOs in the FV1](http://www.spinsemi.com/Products/appnotes/spn1001/AN-0001.pdf)
