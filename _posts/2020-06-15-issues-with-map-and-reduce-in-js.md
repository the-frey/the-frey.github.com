---
layout: post
title: "Issues with Map and Reduce in JS"
description: "A mini rant"
category:
tags: [javascript,clojurescript,functional programming]
---
{% include JB/setup %}

What is my issue with `map` and `reduce` in Javascript?

Let's set aside mutation in Javascript causing issues, because that's a given. Luckily, `map` does at least return a new array.

It really boils down to the fact that in JS you can call `array.map()` and it's a property of the array - the array is not an array, a data primitive, but an object representing an array. The idea of map and reduce is that they operate generically over _data_, not as a property of objects that can use that about themselves. It's like the confusion with objects <-> types - it's wrong to conflate those two ideas as they are different things.

In a language where arrays and objects (what JS calls objects, and other languages would call a dict, hash, or map) are fundamentally data primitives, it's simpler to work with anything that can operate over those collection types and generically compose multiple operations. So it's philosophical at the level of language design as much as it is calling semantics.

Libraries like Lodash are a step in the right direction, because it does to some extent invert the relationship - lodash provides the functions, as if it was a pseudo standard library, and the data types like objects and arrays are generally - but not always, granted - treated as data primitives that are compatible and composable so long as they can be iterated. You see this in the docs with the notion of a `collection` as an abstract.

You'll see this philosophically in the codebase for the serverless Typescript project I'm currently working on a lot[0] - why reify something into an object when you can have it as primitive data? The closer something is to a primitive, the easier it is to compose (in the sense of compatibility of inputs and outputs for functions or chains of functions) although you may have to do more work transforming over various collection types... but that's what `map` is for, right?

The point is that you quickly get down to primitive data values which are portable, easily serialisable and composable, and that's why `map` and `reduce` shouldn't be properties of collections, they should be functions that _take_ collections, and that's what [Lodash](https://lodash.com/) gets closer to right. [Ramda](https://ramdajs.com/) of course gets it even more right - as everything is immutable by default.

Also, it allows you to more closely pass around data that's either scalar or a collection, and then your transformation functions become generally even more reusable. You often end up with a design that requires more thought to change, but is still quite easy to change.

There's another set of interesting things about this - that a `reduce` operation is basically a specific `fold` operation over a monoid, and that transducers and lazy mapping can be used more easily if you're working with immutable data as inputs. 

Why is that important? Well, besides transducers, portable abstract transformation pipelines and whatnot, which is a whole other topic, trivial parallelization. An example - if I have four map ops, upcase, strip some chars, insert a char, serialise, let's say, I can build those into a single pass over the collection, so I save intermediate collections. It can also be done lazily, and it can also be done in parallel. In fact, if it's lazy, it doesn't even have to occur until it's folded - that is, an action that requires the entire collection to be 'made real', or, well, realised. 

That's basically how Apache Spark works in terms of spreading Big Data loads out for parallel processing (if you kind of squint) although they actually do a bunch of fudging around with the data. At the system design level though, it sort of operates like a program that lazily maps over sequences but doesn't execute any operations until it encounters a fold-like action that requires the sequence to be consumed/realized. 

Map and fold are inherent parts of the computation model - heck, they _are_ the computation model (hence MapReduce in another Big Data methodology) and as such, they aren't simply properties of a datatype; they're way more fundamental than that, and data is data - ideally not an object or reified concept as well... although obviously the _shape_ of data can infer a type, like a schema or interface or (etc etc)...

A bit of a ramble, but that's kind of the root of it.

[0] Hopefully I will write and share more about this at some point.
