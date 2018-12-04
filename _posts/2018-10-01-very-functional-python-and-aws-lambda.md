---
layout: post
title: "Very functional python and AWS Lambda"
description: ""
category: 
tags: [python,lambda,serverless]
---
{% include JB/setup %}

So in the last few posts on Python and AWS Lambda, I've been talking through the evolution of a Python serverless project.

Coming from a Clojure background, I wish that I had some of the syntactic sugar of that language, namely `->`, `->>` and `comp`, at a minimum. 

Enter the python `functools` library, and an easy snippet to generate a working `comp` function.

This has two interesting side effects - first, that we can now chain together any number of arbitrary operations so long as:

1. The inputs are the same shape (a list)
2. The items in the list are the same shape (a JSON schema)
3. We rescue from errors and then handle at the end of operations on the list

What's interesting about this pattern is it means that you can think about each Lambda as a set of transformations and then set of Lambdas as conceptually similar to a `comp` - after all, they're basically operating over the same shape of data the entire time.

We have a structure a little like this, FWIW:

```
{
  "payload": {
    "field1": 1,
    "field2": "foo"
    "optional_properties": {}
  },
  "errors": []
}
```

A great side effect of this is that you also get errors persisting between Lambdas. In order to maximise throughput we tend to DLQ errors when they occur, and then monitor those metrics, but it's possible something would error and then be valid enough to cross a system (Lambda) boundary successfully. It's admittedly an edge case, but nice to know we're covered.

My colleague cooked this up as our utility fn, based on the docs:

```
import functools

def compose(*functions):
    def compose2(f, g):
        return lambda x: f(g(x))
    return functools.reduce(compose2, functions, lambda x: x)
```

So then, let's say we want to transform our payload above - we want to flatten out the `optional_properties` into the top level, so we have a series of operations:

- unpack the records from kinesis
- assemble them into a flattened shape
- insert them

We of course would like to accomplish this without breaking the contract of input shape, which should be a list of what we'll call 'records', after the AWS convention for which key they're under in the JSON payload.

To do this, we will use a partial function to apply our configuration to an insert function, but leave an arity of 1 that takes only a list of records.

<script src="https://gist.github.com/the-frey/83cc0c385de0c23ccdd664845db39e6c.js"></script>

Hopefully that makes sense.