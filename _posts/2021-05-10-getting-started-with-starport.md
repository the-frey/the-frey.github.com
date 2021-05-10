---
layout: post
title: "Getting started with Starport"
description: "Quickstart for OSX"
category:
tags: [blockchain,cosmos,starport]
---
{% include JB/setup %}

Recently I've been mucking around with the Cosmos SDK, and its promise of 'Rails for Blockchain apps' doesn't seem far off the mark (so far). It does make you wonder if some of the inherent flaws of Rails are also shared - but that's a post for another day. Hopefully the separation between the 'engine', in this case Tendermint and the ABCI interface, and the application is good enough to mean that any issues down the line can be dealt with. Excitingly, although the official Go SDK is the main horse in town at the moment, several other alternatives are in the works for building projects on top of ABCI. Anyway.

For getting up-and-running fast, you can use the [Starport](https://github.com/tendermint/starport) tool. I've been using it for doing some testnet stuff on a Linux box, and just came to set it up for local development on my Mac. This is more for my benefit really, as the docs are pretty damn good.

At the time of writing, the following three dependencies are required to use `starport`:

1. [Golang >=1.16](https://golang.org/)
2. [Node.js >=14.16.1](https://nodejs.org/en/)
3. [protoc](https://grpc.io/docs/protoc-installation/)

You can then [install via curl](https://docs.starport.network/scaffold/intro.html) on Linux, or on Mac OSX, you can use [Homebrew](https://brew.sh/).

```shell
brew install tendermint/tap/starport
brew install protobuf # if you didn't do this w/ step 3 above
```

Having done that, you should be good to go:

```shell
starport app github.com/<your-username>/voter
```

Once you've scaffolded your app, you might want to [check out the project structure reference](https://docs.starport.network/scaffold/project.html).

To create the entities in your system, you'll want to check out the [type scaffold reference](https://docs.starport.network/scaffold/type.html) before running

```shell
starport type [typename] [field0] [field1] ... [flags]
```

IF you've not gone through it already, then the `voter` [tutorial](https://tutorials.cosmos.network/voter/) is worth walking through.
