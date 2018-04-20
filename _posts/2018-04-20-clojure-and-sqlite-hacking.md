---
layout: post
title: "Clojure and SQLite Hacking"
description: "A quick hack..."
category: 
tags: [clojure,data,sqlite]
---
{% include JB/setup %}

I had to knock together an embedded database as a proof-of-concept the other day, and had a couple of passes through making some JSON queryable via SQL and SQLite (yes, I know what you're thinking - Postgres wasn't an option).

Obviously I've changed the examples from the data types and purpose of what I was doing, but hey. Let's just say it was taking from a wide row and grabbing some particular JSON fields we cared about querying.

Anyway, brewed this up initially:

<script src="https://gist.github.com/the-frey/c465fdfaeb171d456dcead9e892a40f3.js"></script>

Then refactored using `comp`:

<script src="https://gist.github.com/the-frey/73f5a67fc5a2f6bac81dfc519168738b.js"></script>

This means that we can leverage some lazy behaviour to keep file loading under control and deal with larger inputs more gracefully. Finally, we end up with:

<script src="https://gist.github.com/the-frey/ec2344bbbf73446ed846e09ff7a09588.js"></script>

Not too shabby for a quick hack.
