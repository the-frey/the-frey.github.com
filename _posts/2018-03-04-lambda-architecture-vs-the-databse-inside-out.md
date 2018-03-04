---
layout: post
title: "Lambda Architecture vs The Database Inside Out"
description: "Some thoughts after the AWS Builder's Day..."
category: 
tags: [architecture,devops,data]
---
{% include JB/setup %}

# Lambda vs Kappa Architecture

Earlier this week, I went to the AWS Builder's Day in Manchester and followed the lambda track.

First off - if you get the chance to go to one of these events, I'd recommend it. Even if you're not exclusively or extensively using AWS services it's a great taster of what's happening out there at the cutting-edge of Infrastructure-as-a-service, and provocative in terms of giving you the time and space to think about what lessons you can apply to your own architecture and tooling choices.

Either way, here are some thoughts based on the notes that I scribbled.

A lot of it has to do with two broad trends in architecture, and particularly high-volume data architecture. I may call it something different, but we're I suppose talking about the Lambda vs Kappa[1] patterns in some way, shape or form.

In this blogpost, I'll generally refer to the _Kappa_ pattern as _The Database Inside Out_, chiefly so I can contrast with the model of an abstract computer program.

## The Database Inside Out

In the area of large-scale data systems, which is ostensibly the general area in which I work these days, I think the most compelling writing on modern distributed data architectures that I've seen is probably that outlined in Martin Kleppmann's excellent book _Designing Data Intensive Applications_. Although the book itself covers a lot of diverse topics, his approach to thinking about the systems in an organisation fits a pattern roughly described in one of his conference talks that I'm going to coin _The Database Inside Out_.

Essentially, this means conceptually treating your organisation like a **single, abstract database**.

This is based around the notion that if you're using a persistent log to integrate and hydrate data systems at the core of your architecture then you're taking a component based on the transaction log of a database and using it as an abstraction. Taking this abstraction to its logical conclusion means that indeed your organisation can be considered as a database, its operational and analytic data stores as derived data views, and any data-at-rest or persistence to an underlying file system as the persistence mechanism; with of course the caveat that theoretically infinite logs and Change Data Capture can mean your streaming platform can also be your persistence layer.

## The Lambda Architecture

Meanwhile, over in AWS-land:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Interesting that so much of AWS&#39; newer tooling is around foundational CS concepts like lists/queues, state machines and lambdas <a href="https://twitter.com/hashtag/buildonaws?src=hash&amp;ref_src=twsrc%5Etfw">#buildonaws</a></p>&mdash; Alex Lynham (@hipsters_unite) <a href="https://twitter.com/hipsters_unite/status/968515771222626304?ref_src=twsrc%5Etfw">February 27, 2018</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

This is interesting, because when you add AWS Lambda (anonymous functions) Kinesis, SQS/SNS (queues, or lists) Dynamo DB (sort of like a meta hash table), Step Functions (state machines) together, you basically have all the standard library of a simple programming language, and a strong steer towards architecting in a more functional style.

To borrow some ideas from Rich Hickey (go and watch _The Language of the System_, if you haven't already), if you're already treating data as the first class citizen of your system it should be, and thinking about things like schemas, validation, evolving definitions, versioning, micro- or nano-services, then neither _The Database Inside Out_ nor _The Lambda Architecture_ should be counter-intuitive as architectural patterns. 

Your primary concern should be about the flow of data in your organisation, and a pragmatic coupling that allows for rapid development both of new components and products and of replacements for worn out products or components.

By reifying abstract data flows and making sure they are not tied to runtime or use-specific representations at a system-wide level, we can build distributed systems with a much more plug-and-play feel, and this is common to both the _Lambda_ and _Kappa_ patterns.

The difference boils down to implementation details - are you running microservices on virtual instances, containers or bare metal, or are you running hooks provided by vendor tooling? Both require you to have a strong handle on the interfaces in your system where data passes between components.

However, getting back on track, what the AWS-flavour of Lambda amounts to, so far as I can see, is **treating your organisation like a program**, which is perhaps an even greater level of abstraction than that already conceptually presented by Kleppmann and co.

## Wrap up

Obviously different organisations have different scaling needs, different estate sizes and different cost versus performance concerns, and that will surely play into any assessment of hosted solution versus partially-rolled solution. Particularly in an organisation with vastly heterogeneous and long-lived data sources that are either cloud or on-prem, it's easy to see that the Lambda architecture could only hope to solve particular use-cases in a programme of radical organisational change or complete greenfield re-write, but nevertheless, it offers most of the tools you'd need to do incremental replacement or strangle-and-replace for key systems.

However, shape of organisation, willingness to pay for tooling versus devops time and organisational maturity aside, it remains to ask the following:

_Does the shape and business activity of my organisation more closely conceptually resemble a program executing, or an operational data store running?_

The question of whether the transformation and return of input or the storage and querying of state is your key business activity seems key to assessing this.

It's an interesting and subtle thought exercise because of the traditional close coupling of data systems and user-client interfaces, but to pinch an example from the Builder's Day - maybe a media delivery company such as Thompson Reuters is more like a program, and a traditional retailer is more like an operational data store.

Disagree? [Let's argue on Twitter](https://twitter.com/hipsters_unite).

[1] Probably worth pointing out that the log and stream integration pattern in various forms is often grouped under the blanket of _The Kappa Architecture_, which I believe originated as a joke from Jay Kreps, the creator of Apache Kafka. Here's a [repository of info](http://milinda.pathirage.org/kappa-architecture.com/) and Kreps' [original O'Reilly blog piece](https://www.oreilly.com/ideas/questioning-the-lambda-architecture).
