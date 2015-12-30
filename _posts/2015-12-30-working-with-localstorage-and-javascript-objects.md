---
layout: post
title: "Working with LocalStorage and Javascript Objects"
description: "Working with JSON objects and LocalStorage is super awesome."
category: 
tags: [javascript, frontend, localstorage]
---
{% include JB/setup %}

In the last month or so, I've been building a feature that relies on a small but verging on non-trivial LocalStorage-based application that allows users to create new sets of data based on columns from existing datasets. That part isn't important for the purposes of this post; but what's been surprising is just how easy it is to serialise and deserialise complicated datastructures from or to LocalStorage. Granted, using JSON as the interchange format, this is to be expected, but with a unique primary key format in hand (RDF URIs) it was trivial to implement what was, in effect, a mini-ORM, complete with error messages et cetera.

So, anyway - first up, we need to check LocalStorage is present and available. MDN has a useful function for this:

<script src="https://gist.github.com/the-frey/416ff0418fb902cf283b.js"></script>

Then, we're onto the fun stuff. Let's serialise a hash-map, and assume that we have primary keys that are unique; you could achieve this with a numerical key or GUIDs, as per the Ember-data fixture adapter (IIRC) and the Backbone.js LocalStorage library.

<script src="https://gist.github.com/the-frey/1c0907cda892e1dd88e7.js"></script>

Having initialised the data structure, we can then recall items from it with ease:

<script src="https://gist.github.com/the-frey/151adf6d2e4f60af0b9f.js"></script>

It is worth remembering at this point, that if events occur in other browser tabs, they can esure consistent state by always calling LocalStorage to update objects rather than relying on an in-memory mapping or anything pushed into the DOM. As LocalStorage is relatively quick in terms of retrieval, this has not proved a problem in the use case I was exploring.

Though obviously there are limits to what can be done with LS (especially given storage limits et cetera), for small amounts of data storage and retrieval in a user session, it proves rather robust, and in the context of a Rspec test run via Rails, I was even able to write an integration test that saved to and recalled from LocalStorage. Neat.