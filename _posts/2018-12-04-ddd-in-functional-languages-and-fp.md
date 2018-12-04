---
layout: post
title: "DDD in Functional Languages and FP"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Now, I'm by no means an expert in this area, but there was a conversation at work about DDD in FP languages provoked by this great talk by Scott Wlaschin, and I figured I'd write up some thoughts.

https://youtu.be/Up7LcbGZFuo

It seems like Scott's essential insight (which, granted, is a good one) is that DDD in OO languages is marred by the tight coupling, or (sigh) 'complecting' between type and class.

Once you're just modelling your domain with types and then using those in functions, you have a much greater flexibility in terms of how you compose together functionality (see what I did there?)... while still being able to 'noun' your business domain and build up an ontology/bounded context.

It's worth _puts on contrarian hat_ observing though that this applies more in a typed lambda calculus lang like haskell (or F# as a poor person's haskell (is that too mean?)) than it does in ancestral FP languages like ML or Lisp, where you will[0] get data in and data out. In those situations the burden is placed more at the point of access rather than the point of definition.

That's why you see a lot of business domain being written as functions like

```
login->session
account-trigger->email
basket->order
```

Where there's an implicit type at either end. This is I suppose where the schema-based stuff comes in, and that's super interesting in terms of weak pseudo polymorphic typing... but crucially for this discussion, allows you to noun your domain.

i.e. a schema for the hash maps that are of the right shape for `login`, `session`, `account-trigger`, `email`, `basket`, and `order` to use my example above.

FWIW, in Haskell, these might be modelled:

``` 
logIn :: LogIn -> Session
triggerEmail :: AccountTrigger -> Email
placeOrder :: Basket -> Order 
```

I think the schema-based example is interesting though - because there are points in a system where access is the most important interface, e.g.

- Database access
- HTTP API

That's where you leave a system boundary (side effects!) so the types and DDD in this manner can't save you... but schemas can, because that's contract-based testing on read.

Thus it’s an interesting thought exercise to ponder if for example, REST is harder to reconcile with a pragmatic DDD approach than GraphQL.

[0] Mostly.

