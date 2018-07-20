---
layout: post
title: "Structuring a Serverless Python backend"
description: ""
category: 
tags: [serverless,python,lambda]
---
{% include JB/setup %}

## Context

I've recently been working on a Python project that roughly follows the same pattern as that described in my earlier Serverless Stream Platform post. There are some particular difficulties that have cropped up in using Python for real-world Lambdas that I thought were worth notating. First though - some background. 

The Serverless framework is very targeted at Node.js, so for the purposes of this post, let's assume that we're using Python on its own and without the supporting tooling that Serverless can provide. Let's say we have a build box that will deploy our Lambdas.

Thus, we have some key requirements:

1. Local testing of Lambdas
2. Sensible project structure
3. No dependency mismatch between Lambdas in our estate
4. The ability to build and deploy Lambdas manually

The final point is interesting; we might have a QA or a sandbox environment that means we can do spikes, or integration testing before things are ready for prime-time. As we're developing on Mac... or, let's say, that we're not assuming devs will be using Linux locally, we need to be able to build our Python dependencies that have extensions in a Linux environment. Docker is a good fit for accomplishing this, and giving us back a neat zipfile for use via the AWS UI. 

## Project structure

As in my previous post, we want to structure our project broadly around the `producer` and `consumer` concepts if we're talking about using a log as the way that our components talk to each other.

### "But I'm building a web stack!" version

Oh, cool. I'm doing that in a Clojure project elsewhere. You probably just want one folder then. I'd suggest `handlers` is a good name, and then follow the same convention described below.

For example, an index handler (`def index(event, context):`) would be `handlers.index` in the AWS UI and have the folder structure:

```
.
└── handlers
    ├── index
    │   ├── handlers.py
    │   └── __init__.py
    ├── login
    │   ├── handlers.py
    │   └── __init__.py
    └── __init__.py
```

Almost the rest of this post will be the same, just with different paths. The build materials will be identical.

### Log-centric version

Produce a sub-folder under the type (producer/consumer) that the Lambda falls under, and name it the same thing as the handler. This will allow you to find code quicker. 

By naming all the files the same thing, you're basically referring to them as if they were a conceptual namespace, even if they aren't in practice. 

For example, within `producers` we might have two files:

```
.
└── producers
    ├── log_event
    │   └── producers.py
    └── error_event
        └── producers.py
```

This will mean that in the AWS UI, I tag the handler with `producers.error_event`, assuming a handler: 

```python
def error_event(event, context):
  # do stuff
  return {"error": "oh no!"}
```

Given that shape, you can see that to find the handler, I then go to `producers/error_event` from the root, and my lambda's python file will be there.

Obviously, at every point in the hierarchy, you'll need an `__init__.py` file as well, so be sure to add that in:

```
.
└── producers
    ├── log_event
    │   ├── producers.py
    │   └── __init__.py
    ├── error_event
    │   ├── producers.py
    │   └── __init__.py
    └── __init__.py
```

This will allow you to include these files from a test file. Let's say we want to test a function, `serialize_error_event` in `error_event/producers.py`, in our test file we'd write:

```
from producers.error_event.producers import serialize_error_event
```

Then we could test by using, at the root of the directory:

    python unittest -m

Remember that your tests directories will need `__init__.py` files adding manually as well!

## Style

There's a temptation, because Lambdas are so small, to not commit to any particular programming style within them. However, as a functional programmer, I have to say _something_ whiny about side effects. Here goes.

You'll have a better time if you follow [functional core, imperative shell](https://www.destroyallsoftware.com/screencasts/catalog/functional-core-imperative-shell), because although a lot of the most useful Lambdas involve side effects, for example, writing to RDS, the pain of these can be reduced and things like transformations can then be easily tested.

Some ideas that will help you write consistent Lambdas:

- The handler is like the imperative shell, and is responsible for side-effects and IO
- Mutable state is bad... that's just something I wanted to say
- Referentially transparent or 'pure' functionality should be broken out into functions
- These functions should then be unit tested

You'll find it hard to test your handlers much of the time if they're side-effecting, as you'll have to basically bootstrap a whole integration environment... and that's the job of pre-prod, right? That's exactly why you're writing Lambdas - to get away from all that red tape!

Well yes, quite, my functional friend - and you'll find that even in Clojurescript for example that handlers are often a pain to unit test in isolation.

Okay, so from our example project above, let's assume that `error_event` is firing the error as JSON into Kinesis. Let's write a side-effecting handler to read that from Kinesis.

<script src="https://gist.github.com/the-frey/3229ab1c7674c726f69c6e17d8519283.js"></script>

So in this case, there's only one bit we can break out - as I don't see much point in testing that a Postgres library can connect to a Postgres DB (you would hope that it would have its own tests, given how widely used it is). So now we could test that little transformation function in isolation.

In real-life cases, I've seen slightly more complicated logic for wrangling and transforming, and being able to unit test the transformation steps can be pretty useful. Then, if something goes wrong, in this example, you can isolate it down to one of three things:

- The handler (network issues)
- The database (connection or env var issues)
- The database (write or read issues)

The final one there is very common - if we remember our example Kinesis events with all the cruft stripped away:

<script src="https://gist.github.com/the-frey/2defc8e7f062681c11371d7024adf987.js"></script>

Quite often when testing Lambdas in the UI, we'll drop a blob of JSON like this into the 'Test' pane, and then run it. If it successfully saves to RDS then in our Python example above, we'd have hardcoded IDs. If in your table you've specified unique IDs as a constraint, then your test will work the first time, then you'll have to clear the table using `psql` before you can test again.

Alternatively, and what I favour - you can write a Lambda that clears the table, and one that reads all the contents of the table. What better way of demonstrating your Serverless prowess than eliminating the need to SSH on to your box or bastion and spin up `psql`?!

## Project setup

With one level of nesting, your project should look something like:

```
.
├── Dockerfile
├── README.md
├── __init__.py
├── build.sh
├── consumers # <-- folder for all consumers
├── producers # <-- folder for all producers
├── requirements.txt # <-- for all!
├── target
└── test
```

## Building

So as discussed above, we have a few utilities:

```
.
├── Dockerfile # <-- builds and dumps a zip into target
├── README.md
├── __init__.py
├── build.sh # <-- triggers docker
├── consumers
├── producers
├── requirements.txt
├── target # <-- output dir
└── test
```

Make sure you have docker installed, and `/target` is in your `.gitignore`.

Use this `build.sh` file to build namespaced lambdas in your `target` directory.

<script src="https://gist.github.com/the-frey/a48f63d15a3a7f8327b648999a049fe6.js"></script>

It can be used like:

    ./build.sh error_event ./consumers/error_event

The first argument is your handler name, and your second argument is the path to the folder in which that Lambda handler can be found. 

> Note that the requirements.txt at the root of the repo will be used for all Lambdas.

Now I think about it, `error_event` should probably be `error_source`, to match `error_sink`, but hey. I've written a lot of words and I will update this later. Anyhoo, here's the Dockerfile:

<script src="https://gist.github.com/the-frey/c99abb1f56bc8125e6ee176c9176853d.js"></script>

## Misc

Finally, it should be pretty simple to set up CI. For this kind of project, I strongly recommend adopting a [trunk-based development](https://trunkbaseddevelopment.com/) model, or go extreme and just commit only on Master. Lambdas are great for pushing your working practices towards Continuous Delivery, and you should embrace the change that entails while you're adopting this technology to get as much pain over in one go.

CD requires good CI tooling, however. I prefer Travis, but I've been building these out in a Gitlab environment. Gitlab CI is pretty trivial to set up. Just put a `.gitlab-ci.yml` file in the root of your repo with the following:

<script src="https://gist.github.com/the-frey/9e215bbd9b0195fd77143b6192c55730.js"></script>

That's it. Are you doing it differently? Hit me up on Twitter - I'd love to see how you're developing and deploying your Lambdas.

### p.s.

If you're really serious about Lambdas, a lot of the mindshare is in the Node space, so seriously consider using ES6, or something like CLJS that compiles to JS. Be sure to check out [Serverless](https://serverless.com/) before allowing for the constraints discussed in this post.

I'll post some follow-ups soon with my experiments in CLJS and Serverless, I promise.

