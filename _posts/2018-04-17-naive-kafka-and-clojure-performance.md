---
layout: post
title: "Naive Kafka and Clojure performance"
description: "Hardly rigorous, but I've written it down anyway..."
category: 
tags: [kafka,clojure,streaming]
---
{% include JB/setup %}

As per the code in my last post, I've been messing around with Kafka.

I'm reading an input file that's over a GB in size, with about 25.5 million rows in lazily, and then performing some transformations on it before handing it over to Kafka.

There's also a consumer running in the terminal, to eyeball what's happening.

Looking at serialization strategies, I did a quick compare of nippy versus JSON (via clojure's `data.json` library). I also threw in Jsonista, as their benchmarks versus Cheshire - already faster than the core JSON library - were impressive.

Here's the slightly noddy code:

<script src="https://gist.github.com/the-frey/0f99c652519185755f7726cbe049f6ea.js"></script>

Just on my laptop, but with a couple of runs against a single broker, with a consumer reading every message off (in the terminal), I saw the following.

These were run a few times to make sure the times were +/- 10%, but only the final run is recorded here. I've rounded them too.

*JSON*

- ~25m rows of data from file as strings
- Assoc'd into the `:body` key of a new hashmap
- Serialized and sent to Kafka
- ~551 secs run time
- ~40k msgs/s

*Nippy*

- ~25m rows of data from file as strings
- Assoc'd into the `:body` key of a new hashmap
- Serialized and sent to Kafka
- ~295 secs run time
- ~80k msgs/s

*Jsonista*

- ~25m rows of data from file as strings
- Assoc'd into the `:body` key of a new hashmap
- Serialized and sent to Kafka
- ~250 secs run time
- ~100k msgs/s

_Although the time on this wobbled around a bit, the quickest it ran in was an astonishing ~168s, making it ~150k msgs/s on that run. Whoa._

It's worth noting that using non-lazy io blows up. The lazy hype is real, yo!

You could, I suppose, poll for a set number of lines and then you could batch and parallelize, so maybe I will try that to see how it performs against using `comp`'d functions.

Hardly that scientific, but it's something.
