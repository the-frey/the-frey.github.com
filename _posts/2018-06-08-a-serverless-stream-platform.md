---
layout: post
title: "A Serverless Stream Platform"
description: ""
category: 
tags: [systems,data,serverless]
---
{% include JB/setup %}

On a number of [posts like this one](https://www.oreilly.com/ideas/apache-kafka-and-the-rise-of-the-streaming-platform), the makers of Apache Kafka use the term _Streaming Platform_ to refer to Apache Kafka and the architectural pattern that puts it at the centre.

Kafka isn't right for every business or project, however, and you might instead look to managed services like AWS Kinesis - a log-based stream served with producers and consumers - to build out similar architectures, without having to maintain your own infrastructure and tune your own clusters.

Like anything, there's a bunch of trade-offs to be made around this. Both have good access controls, stream-based querying, and a host of other features. The real differences are in retention and delivery guarantees. Kafka offers idempotent/exactly once delivery, with a host of lesser guarantee levels, while Kinesis requires you to de-dupe at the consumer end. Secondly, Kinesis' max retention period is a week, or 168 hours, with a default of 24, while Kafka can on paper store an infinite log of data, and some organisations like the BBC and New York Times take advantage of this feature at moderate scale.

So anyway, I'd been thinking of whether you could conceptually group your data integration layer, architected as a streaming platform, into a [Serverless](https://serverless.com) application. Serverless - with a capital 'S' - is sort of like Kubernetes for Functions-as-a-Service,and allows you to deploy a set together, or individually, as well as group them together with some configuration and run them locally. An alternative is [AWS SAM](https://github.com/awslabs/aws-sam-cli), but that's obviously more tightly locked-in to the AWS platform.

So anyway, [here's a repo](https://github.com/the-frey/serverless_stream_platform).

You should be able to clone the repo and get it running pretty quickly:

    npm install -g kinesalite
    npm install
    npm run offline

Then curl the exposed endpoint at `localhost:3000`. At the moment, it just drops an event on the stream and then logs it at the other end. There's some scaffold to mimic the firehose that you'd set up in the AWS environment in `utils/local-firehose.js`. Like all development scaffolding, it's a bit gnarly.

```
curl -v -X POST \
     -H "Content-Type: application/json" \
     localhost:3000 \
     --data '{"foo": "bar"}'
```

The structure is as follows, heavily inspired by [this great blog post on Serverless with Kinesis](http://blogs.lessthandot.com/index.php/enterprisedev/cloud/serverless-http-kinesis-lambdas-with-offline-development/):

```
.
├── LICENSE
├── README.md
├── config
│   └── env.yml
├── consumer-functions.yml
├── consumers
│   └── consumers.js
├── package-lock.json
├── package.json
├── producer-functions.yml
├── producers
│   └── producers.js
├── serverless.yml
└── utils
    ├── env-yaml.js
    ├── local-firehose.js
    └── stream-setup.js
```

Things to note:

- The `env` file is read in by `utils/env-yaml.js`, and allows you to specify some env in a fixture file.
- The main configuration is in `serverless.yml`.
- The producers are in `producer-functions.yml`, and the consumers in... you guessed it!
- At the moment as there's only two functions, they're somewhat dimly placed in files named after their function (produce or consume) - ultimately, you would have more functions in these folders, with the filenames updated appropriately. You'd then enumerate these in their respective `yaml` files.
- I'm using a kinesis wrapper I wrote mainly to get to grips with ES6 again. I originally wanted to do something more data-oriented and Clojure-like, and eventually concluded that I should probably just go with the JS grain. So, if I were you, I'd probably just fall back to the AWS JS SDK for this.

If you want to understand what's going on, I'd suggest a trivial amendment would be to try and get it working so that in the body of your request you can specify which stream to put the message on - this could be a client id for a multi-tenanted system or SaaS product, for example.
