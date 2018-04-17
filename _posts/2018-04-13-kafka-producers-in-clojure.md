---
layout: post
title: "Kafka Producers in Clojure"
description: "Some quick notes"
category: 
tags: [kafka,clojure,data]
---
{% include JB/setup %}

I've been playing around with Kafka for a while, but only recently started work in earnest on an implementation in the wild using Clojure. It turns out that to write a little wrapper around the Java library is pretty trivial, and for the use case of serializing arbitrary hashmaps it's relatively terse and simple to boot.

During some stress-testing today, on a single Kafka broker on my laptop with no performance tuning I was able to get about 140k msgs/sec throughput as strings from a multi-GB file with 25m rows, so for most use-cases that's going to be quick enough.

I'm working on a micro-library called _needlecast_ to bootstrap JSON and nippy serializer/deserializers and Kafka producers and consumers, which hopefully I'll have a first pass at this weekend.

Here's all the functions you'd need as well as a lazy example to write line-separated data from a static file (it's just posting the whole line - hopefully it's obvious where you would change this to do parsing/transforms etc).

This needs a refactor, but here's the main ns:

<script src="https://gist.github.com/the-frey/a414f18f3853ee6feeab2a83d235a00c.js"></script>

Here's the `project.clj` you'll need to run the snippet:

<script src="https://gist.github.com/the-frey/c5140b4a602e6e67fdb603941f3ae35b.js"></script>

Here's a travis file as per my other post:

<script src="https://gist.github.com/the-frey/f1cc6e6531631578a4b6a903a7ae9195.js"></script>
