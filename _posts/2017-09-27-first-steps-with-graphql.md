---
layout: post
title: "First steps with GraphQL"
description: "Maybe I'm thick, but it took a lot of effort to work out passing lists..."
category: 
tags: [graphql,data,clojure]
---
{% include JB/setup %}

I've recently started hacking around with GraphQL, and coming from a linked-data world, the examples all being fundamentally resource-oriented, sort of like a REST-on-steroids has not really filled the use case I was seeking. After some poking around with the excellent graphql-clj [starter kit, which I've extended with more examples](https://github.com/the-frey/graphql-clj-starter) and some questions here and there, I've managed to get some cool stuff working. Extending query types to send complex objects like hashes and lists to your back-end turns out to be relatively trivial, and the powerful list-processing powers of Clojure are there to pick up the slack once your data arrives.

Although it's been fun working with [graphql-clj](https://github.com/tendant/graphql-clj) I think I'd be tempted to move to using Walmart Labs' [Lacinia](https://github.com/walmartlabs/lacinia) as it allows for EDN schemas. Parsing strings for schemas is somewhat janky in my book, and I'd like to avoid it if possible.
