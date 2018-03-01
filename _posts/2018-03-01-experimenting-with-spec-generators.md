---
layout: post
title: "Experimenting with Spec Generators"
description: "Hammer of the hacks"
category: 
tags: [hacks,clojure,spec]
---
{% include JB/setup %}

As anybody who's had the misfortune to be near me at work or a meetup will know, I'm a big fan of Plumatic Schema and the Compojure API library, so I've been keen for a while to dig a bit deeper with Clojure's new spec library.

A use case crossed my desk for having to hammer a bunch of flattened records into a NRT store, so I thought about whether there was an opportunity to combine a spec for conforming code that would run in production with a tool that could chaos monkey-style generate me loads of gibberish to smash into my data store.

As it turns out, spec on its own generates random noise to the point of empty strings etc, so depending on how 'real' you want your generated fields to look, you might have to do a fair bit of hacking.

Now, pedantically I think I might observe that at this point spec is neither (in my opinion) simple nor easy to use... but it's interesting enough to be useful here.

Below is a snippet adapted from the examples in the [spec documentation](https://clojure.org/guides/spec) to show a simple Person spec built up and primed to generate email addresses that pass a lenient validator.

I don't think I've got the generation of the `_id` field quite right yet, but for now you can see how passing a generator and then discarding its value means you can hijack it - maybe to put in a mongoDB BSON ID or what have you. 

When I've worked out the proper way of doing that, I'll update this post.

<script src="https://gist.github.com/the-frey/aa4d833a1784f8ad126d6e7016de5a25.js"></script>

Run the example above with:

    (gen/sample (s/gen ::person))

This will generate (I think) ten by default. You can increase the number quite simply:

    (gen/sample (s/gen ::person) 20)


