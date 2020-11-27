---
layout: post
title: "fp-ts and Do"
description: "Some musings on using Do notation in Typescript"
category:
tags: [javascript,typescript,functional programming]
---
{% include JB/setup %}

After a brief spike, we've started converting over most of our handlers to use the `Do` notation that's included in the `fp-ts-contrib` package.

There's an overview of [do notation here](https://gcanti.github.io/fp-ts-contrib/modules/Do.ts.html), or Paul Gray's much more comprehensive summary available [here](https://paulgray.net/do-syntax-in-typescript/).

In a nutshell, most of our operations are asynchronous, so in moving to a monad-based implementation, we were going to be doing a lot of lifting already written async functions into a monadic context. In `fp-ts` terms, async operations are governed by `Task`. A `Task` that might fail is a `TaskEither` and helpfully it's trivial to lift an `Either` into the `TaskEither` context. Assuming you have `TaskEither` imported as `TE`, it's as simple as `TE.fromEither(<your-async-fn(args)>)`.

To give the example of an endpoint that returned a `User` stripped of any PII or sensitive data, we came to this in our initial spike:

<script src="https://gist.github.com/the-frey/c766fc8996a164417e013e6516aaa643.js"></script>

While my friend that is a co-organiser of Lambda Lounge MCR pointed out that if you don't care about the intermediate binding steps in the `Do` notation, the following is basically equivalent:

<script src="https://gist.github.com/the-frey/668335eee9910c6e4a6b8d396e033945.js"></script>

You'll notice some of the stuff in this example isn't typed - our events and whatnot are actually typed in the codebase with something less aggressive than the AWS/types package so that we can pass minimal request maps from integration tests and treat our handlers like simple functions that accept a hashmap as input - see my [other blog post](https://the-frey.github.io/2020/06/15/functional-serverless-typescript) for some more info about how this thrashes out in practice.

From there, we realised that handling the database connection would be more effectively done using a `Reader`. Luckily, there's a `ReaderTaskEither` [in the package](https://gcanti.github.io/fp-ts/modules/ReaderTaskEither.ts.html).

An example database function for us looks something like this:

<script src="https://gist.github.com/the-frey/ad4ac15e5980dcba3897bfd918e87d44.js"></script>

You'll notice that the error type and the entity type in these examples all follow a similar shape to an HTTP response. Originally we had an HTTP response type with `error|success` subtypes as the `left|right` branches in an `Either`, but it felt weird for so many parts of the stack to have 'knowledge' of that type. We ended up replacing it with a generic hashmap type with an optional status code in the error case.

Although these types are mostly equivalent to an HTTP response type, they're fractionally more generic, and we can use them elsewhere. Hey, it's a work-in-progress.

If Typescript (and Javascript) were better at handling arrays, we might have coined a variant of `[type entity]` to describe these, but in the TS world an interface to govern a hashmap is easier.

<script src="https://gist.github.com/the-frey/bf2de12086bb09141b2fe27c7e5317e4.js"></script>

We're gradually enumerating all the allowed types in the top `entity` key, but it's taking a while to catch all the possibilities, so even in the real codebase it has an `any` fallback. Which... isn't great, but we have to compromise for the sake of continuing to deliver features at a reasonable pace as we re-write.

After that, we refactored out the db conn into a `System` map, which would allow us to pass around env and other connections as well in the `Reader`:

<script src="https://gist.github.com/the-frey/cbdb1f6012f73b638f43617c44888963.js"></script>

Which we could then use like so:

<script src="https://gist.github.com/the-frey/7a18b6a2c70019a3d60b68608e353a62.js"></script>

Finally, by using the side-effecting `.do` call (and `.doL` if bindings are needed), we're able to open transactions over calls, resolving them in the final `fold` operation.

These functions use the Reader context that's already been used for the other operations, and just return the op inside (in this case, a response).

<script src="https://gist.github.com/the-frey/c9a512bef5ac4031fee8583baa1cf296.js"></script>

This means that you can get to a very reusable place where you're able to use nested bindings and fluently control your error states and validation steps like slotting together Lego.

<script src="https://gist.github.com/the-frey/8ccfcfc44b966451aa2857f514f54073.js"></script>

The types on line 25 get pretty crazy, but you can always break these out into their own line with a `type` declaration if they become either:

- completely unmanageable
- need to be used in a union, e.g. `E.Either<t.Error, dbFnReturningUser | dbFnReturningGroup>`
- need to be used in an intersection

Interestingly, the case for unions is usually that the function _returns a function_ - such as the case where the arity might differ, for a case in which a database function selects by multiple ids depending on context:

<script src="https://gist.github.com/the-frey/12f06734fb020488439e6bec44f2a9ac.js"></script>

This allows you to go `E.Either<t.Error, idDbFn | idEntDbFn>` which is a bit more readable in the longer run I think.

As I've worked with the `cats` library and its `mlet` [form in the past](https://funcool.github.io/cats/latest/#mlet), this style of almost lisp/`let` style binding really appeals to me as a way of composing behaviour in a really neat and terse way.
