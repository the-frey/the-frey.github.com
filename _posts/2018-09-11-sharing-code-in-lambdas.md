---
layout: post
title: "Sharing code in Lambdas"
description: ""
category: 
tags: [lambda,serverless,python]
---
{% include JB/setup %}

Just a quick one, this. If you're building out serverless components, you might find yourself duplicating code. On the whole, it's not worth getting too het up about a little bit of repetition - small snippets in several places aren't the end of the world. 

However, if like me you tend to use environment variables as switches to configure additional services for your Lambdas, you might find yourself in possession of a common set of bootstrap functions, which really should be refactored. 

I've seen people suggesting that [a service is an appropriate sharing pattern](https://hackernoon.com/aws-lambda-how-best-to-manage-shared-code-and-shared-infrastructure-827bed395eb7), and for a certain shape of problem, that's certainly true.

However, for most, the use-case will be more simple, and the options therefore are:

- Publish your shared code as a library
- Work out how to inject shared code into a build

The first option is solid if you have good control over your environments and decisions, while the second is probably preferable to having to faff around with private repos, builds and dependency issues. 

The second option will be easier in most cases where you're not a lone developer and where you want code colocated in the same repo, so that's the one we'll focus on here.

Recall that we can navigate a folder structure in our lambdas, and upload a bundle of code including subdirectories. Thus, we just need to make sure our code is accessible locally - for our development and testing, and then bundle any shared code as part of our build process.

Using the Python example from [my other post](http://the-frey.github.io/2018/07/20/structuring-a-serverless-python-backend) we might do the following, adding a `utils` folder with our bootstrap functions in:

```
utils
├── __init__.py
└── aws_utils.py
```

Using the `get_db_creds()` example setup function from the other post, we could refactor that into the new namespace, and then reference it in our main Lambda namespace like so:

    from utils.aws_utils import get_db_creds()

Though, actually, after pulling out the bootstrap, we'd probably be left with something like:

<script src="https://gist.github.com/the-frey/e39e72bfe010eddd352be560f7eb53e3.js"></script>

Then, we need to update our Docker build, so that the `utils` folder is packaged with our handler namespace:

<script src="https://gist.github.com/the-frey/b6c83a38f0b58684c86a57b2ba8682ee.js"></script>

The Dockerfile also now uses Pipenv, a better way of building Python projects.

For reference, a Pipfile, as it's a good idea to move over; you get npm-style task running (for say, excluding tests, as per in this example) and a lockfile: 

<script src="https://gist.github.com/the-frey/4ee4785d193a71fb6b711f1ff99bbd57.js"></script>

Then we're done.

