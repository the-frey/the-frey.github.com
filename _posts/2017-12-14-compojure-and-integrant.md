---
layout: post
title: "Compojure and Integrant"
description: "Here's how to set up Integrant for a Compojure or Compojure-api project..."
category: 
tags: [compojure,integrant,clojure]
---
{% include JB/setup %}

Recently I've been playing around with some compojure-api stuff for some side projects and alphas and decided somewhat foolhardily to try moving away from [Mount](https://github.com/tolitius/mount) as my component management tool. I like Mount; despite some drawbacks of the pattern, modelling databases as singletons clicks with me logically and as I don't tend to run tests from the repl, I've not had the same pain points around testing and reloading that many have had with it.

Anyway, integrating [Integrant](https://github.com/weavejester/integrant) was a bit of a pain, but here's how you do it:

- You make sure that your databases are modelled as components
- Any components that need access to the database reference your database component
- That dependency is then injected and can be accessed by code in that closure
- Repeat for all your components

NB: it's worth saying at this stage that choosing [Duct](https://github.com/duct-framework/duct) as your web toolkit and using [integrant-repl](https://github.com/weavejester/integrant-repl) will allow you to adopt a reloaded-style workflow more easily than integrating with an existing project.

You can see that this is a pretty different pattern from just dereferencing a var, and we'll see how you can set that it up to work in that way in development in just a sec.

So, for a simple compojure-api project with just a web stack and database, we might have the following config file:

<script src="https://gist.github.com/the-frey/a9c56b009cbd01c8c27ec74740c0c371.js"></script>

We then write initializers for Integrant so it can make components out of our connections and depedencies. To componentize our web stack, we simply wrap the `def` in a closure and make it a `defn`.

<script src="https://gist.github.com/the-frey/bd66320d5c7b13a001f881cc69f7851a.js"></script>

Finally, if you want to manually be able to reference your system and start and stop it in development, you can use something like the below, although checking out `integrant-repl` is probably the best call.

<script src="https://gist.github.com/the-frey/812c91c91dbd82129bf7ee131b3dda03.js"></script>

I'll hopefully do a post soon on how to prep this for production use.

Update: I've added some notes on getting integrant-repl set up for dev <a href="/2017/12/18/integrating-integrant-repl-with-an-existing-compojure-project">here</a>.