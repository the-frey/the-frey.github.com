---
layout: post
title: "Mock ring requests"
description: "Mocking ring requests using a minimal map"
category: 
tags: [testing,compojure,ring]
---
{% include JB/setup %}

Most of the time, if you need to test a Compojure API, inside a Ring project or inside a Luminus template, your best bet is `ring.mock.request`; it's a great library and I've written about it elsewhere. However, for specific cases you might need more granular control over formatting, over content-type or accept headers and it's sometimes necessary (especially if your API or backend is very pendantic) to special case things, so it's useful to know what a minimal request map looks like.

In the example below, we're encoding a request body as JSON, to avoid the default encoding of `www-form` and also specifying the accept and content-type headers.

There's an assumption made that we have either the whole app, or a set of routes defined as `app` in the `handler` namespace that's required in the ns declaration.

<script src="https://gist.github.com/the-frey/070b22f17ba544ba992205f725f29d7c.js"></script>

I've found this particular snippet useful for special cases, and it's easy to see how it can be broken out and made more reusable. 