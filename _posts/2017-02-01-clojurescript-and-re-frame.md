---
layout: post
title: "Clojurescript and Re-Frame"
description: "For once, something that might be up to the hype..."
category: 
tags: [clojurescript, react, reagent, re-frame]
---
{% include JB/setup %}

More to come on this later, as I'll probably share some code and do a little walkthrough of how it works, but I've recently had the opportunity to build a small demonstration application with a couple of requirements. I chose [re-frame](https://github.com/Day8/re-frame) to build the app and I thought I'd write a really quick blog with some thoughts.

## The app

So the app:

- Collects records via an API and creates an Elasticsearch index (Clojure)
- Has an API/Search routing layer (Clojure/liberator)
- Has an Javascript SPA for a frontend

After building this, I would say that I'm likely to use re-frame again. For balance, though -

### I like

- Everything has a place; one of my favourite things about frameworks is having a reliable structure. In particular, by using a Luminus template everything kind of had its right place without some of the 'magic' you get in full-blown frameworks.
- How the API gives a consistent flow to event management, which I've found actually does reduce mental overhead. It might not be as low overhead as vanilla JS but it certainly isn't the headache it could be.
- How easy it is to adapt to your specific use case and the shape of your API, while staying relatively terse and idiomatic.
- The interaction between the server and front-end via AJAX is more painless than other frameworks I've tried. It seems to hit a good middle ground between having an established way of dealing with state and events and letting you do your own thing with managing that state (via DB or user interaction or sever calls or what have you).
- Clojurescript quickly has won me around - developing with it feels better than Javascript or Coffeescript, certainly.
- You don't need jQuery for DOM manipulation (I know this is obvious, but worth repeating).
- The hot reloading and workflow with figwheel is quick and solid, especially if you're already used to some combination of emacs, cider and mount, say.

### I dislike

- I can see situations where beyond trivial examples I might have to write a fair bit of glue code. This is an issue with frameworks in general, and I can see that it could happen here.
- There's still a bit of magic about how it all works - the way you hang your code off the API and it 'just works' is a bit disconcerting until you dig deeper.
- Being careful with when and where you deref atoms etc can lead to subtle bugs and annoying 'which part of this is broken?' issues. Caveat - breaking up templates helps _a lot_ with this.

Okay, these are all minor gripes and they're more about me than re-frame, so yeah. You should probably give it a go, is what I'm saying.
