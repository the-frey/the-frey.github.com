---
layout: post
title: "Live code JS lambdas with Scratches"
description: ""
category: 
tags: [lambda,js,aws]
---
{% include JB/setup %}

I've been playing around with AWS Lambdas in nodejs recently for a project, and wondered if it was possible to emulate a repl-style editing experience, seeing as most of the time your code is all loaded within one file or namespace.

A friend pointed me toward the [Scratches editor](https://github.com/hxoht/scratches), which I've found works quite well, although I've got some stuff to work out.

You can attach it to your current working directory and require what you need, then get hacking.

So you can do something like:

<script src="https://gist.github.com/the-frey/48b8e498132ad176e55512655c11bc24.js"></script>

To make that into an actual Lambda is pretty trivial from there:

<script src="https://gist.github.com/the-frey/62a3da4f40d70d22674f2af01d6f30cf.js"></script>

Sorry about the weird indentation, I think that gist is a bit messed up.

Anyway, here's it in action. Nicely light-table like:

![Scratches editor](https://raw.githubusercontent.com/the-frey/the-frey.github.com/master/assets/images/scratches.png)