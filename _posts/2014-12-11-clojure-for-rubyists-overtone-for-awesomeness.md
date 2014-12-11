---
layout: post
title: "Clojure for Rubyists, Overtone for Awesomeness"
description: "Learn Clojure, it's cool. Then learn Overtone, it's cooler."
category: 
tags: [clojure, ruby, live coding]
---
{% include JB/setup %}

At [Swirrl](http://swirrl.com) creating linked data has always been a bit of a pain point. RDF, be it in `.ttl` format or otherwise, is quite a lot of hassle to shoehorn, and messy data didn't help. Back in May, armed with a mixture of Ruby, the `roo` and `rdf` gems and a beaker of oxblood I was just about able to create some linked data, but it was by no means a smooth process. 

Those were some of the smaller datasets we have to work with, and over and beyond the ETL process of transformation and RDF generation there's another problem with Ruby - it's quite slow. That isn't the end of the world in a web application where developer productivity is expensive and consistency helps teams to work (plus libraries, deploy environments, blah blah), but when it's a case of 'write the script, run the script' it needs to complete its run as quickly as possible. 

My colleague Rick who runs Manchester Lambda Lounge had built a proof-of-concept ETL library called [Grafter](http://grafter.org), using Clojure, and so we've gradually been moving over to that and increasing the number of Clojure components we use. 

As part of the ArtsAPI project that I'm currently working on, I got the chance to learn Clojure and use Grafter in anger to parse and convert some pretty large (multiple hundred MB/multiple GB) `.mbox` files. It was pretty painful in places, and I was very glad to have Rick about to quiz about some of the Java (eek!) interop stuff as well as the joys of emacs. Perhaps at some point I'll abstract out the clojure wrapper for the Java `mstor` library that we wrote (of course with Rick refactoring and fixing some astoundingly ungraceful Clojure on my part) and put it on github, but for now, you can just see the full tool on my page if you're interested.

So, Clojure. It's been fun. Here's a bullet point list of some stuff that I've learned about learning it (learn-ception, if you will): 

- Learn emacs. The community supports it and you'll be able to get help more easily. Most of the bolt-ons you use to build an environment will be emacs packages
- If you _really_ don't want to learn emacs, then Cursive and IntelliJ is probably the way to go, but if emacs is dangerously eccentric, then IntelliJ is the editor equivalent of a cubicle. Don't say I didn't warn you. 
- When people say there are similarities between Ruby and Clojure, they basically mean that Keywords look like Ruby symbols; `:foo`. I'm only partly trolling here. 
- Once you start to get the hang of it, the brackets disappear, and you don't notice them.
- Speaking of brackets, learn paredit. It's kind of a pain at first, but quickly it does actually become rather fun, like the Clojure tutorials say.  
- Thinking in terms of list-like abstractions means you fight the language a lot less... this one is very much a work in progress for me though.
- It's a small language... with lots of included macros. This means that you actually need to learn a fair bit to be effective using the language, so don't believe the hype too much.
- It is elegant. I've had some exposure to Scheme, and when people say Clojure is a 'modern lisp', they mean something very specific - it feels like a very different beast, and it's clear that Rich Hickey has thought hard about what language features to pinch from where, and what to take inspiration from.

Anyway, that's enough Kool-Aid. 

One of the fringe benefits of learning Clojure is that there's an awesome SuperCollider wrapping library called Overtone. If you've gone through the pain of getting a viable Clojure dev environment set up for emacs, then congrats - you have a live coding environment too! There's also an [emacs config from one of the maintainers that's pretty cool](https://overtone.github.io/emacs-live/), but if you carefully guard your `.emacs.d` as I have already learned to, then you'll probably not want to jump in and use it right away if you're already an emacs user. 

Anyway, getting started with overtone is super simple. You can follow along with the repl steps in a cider repl if you prefer, or hack away in the terminal.

    lein new overtone-scratch

Then, in the `project.clj` of the created project...

    (defproject overtone-scratch "0.1.0-SNAPSHOT"

    :description "FIXME: write description"
    :url "http://example.com/FIXME"
    :license {:name "Eclipse Public License"
              :url "http://www.eclipse.org/legal/epl-v10.html"}
    :dependencies [[org.clojure/clojure "1.5.1"]
                   [overtone "0.9.1"]])

At the moment, I've mainly been playing with simple beats, live coded drones and the like. I'm not sure what I can show easily in a blog post, so perhaps just the basics of synths and drones...

First, start a repl. 

    lein repl

    user=> 

Then, load Overtone... 

    (use 'overtone.live)

From the getting started guide:

    (defn note->hz [music-note]
      (midi->hz (note music-note)))

Then you can use this to play with some simple synths: 

    (definst low-a-drone [] (saw (note->hz :A2)))

    (low-a-drone)
    ;;=> #<synth-node[loading]: user/low-a-drone 348>

Or import the drum sample library,

    (use 'overtone.inst.drum)

Which will allow you to call random pieces of the kit

    (snare)
    ;;=> #<synth-node[loading]: overtone.inst.drum/snare 637>

Either way, I've only scratched the surface, but I already like it a _lot_, and will definitely be rolling it into the band stuff I'm writing at the moment. Not sure quite how yet, but there's almost too much expression on offer here, so I'm sure something will present itself.