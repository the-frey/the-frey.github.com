---
layout: post
title: "Clojure reducers"
description: "Some very quick notes"
category: 
tags: [clojure,reducers,parallelism]
---
{% include JB/setup %}

I've been throwing together some notes on functional programming in Clojure for a small presentation at work, and I thought it would be useful to show how even in a contrived, noddy example it was simple to bring reducers - and thus improved performance - into the picture. 

<script src="https://gist.github.com/the-frey/f2f7f3e59f45b0e75502e72e256d890f.js"></script>

Bringing in reducers is simple:

<script src="https://gist.github.com/the-frey/76d76ddc1748f7b7ef8bc2bb8672bf8f.js"></script>

Then switching out `(into [])` for `foldcat` is trivial:

<script src="https://gist.github.com/the-frey/cbd47ab6e3b576292115145eb8aafaef.js"></script>