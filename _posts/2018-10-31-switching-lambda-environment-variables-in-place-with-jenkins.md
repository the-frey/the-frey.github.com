---
layout: post
title: "Switching Lambda environment variables in place with Jenkins"
description: ""
category: 
tags: []
---
{% include JB/setup %}

If like me you've been deploying Lambdas using Terraform, and then switching in their configuration and code later using Jenkins, you might wonder how you can manage secrets. 

In the first instance, try to use guessable names et cetera - the example database in the script below is the purpose of the db `analyticdb`, followed by the environment it sits in (because we're doing modular Terraform on a per-environment basis).

For some things that won't be possible. In this example, we've got an SQS instance we're using as a Dead Letter Queue in case updates to RDS fail. 

This queue URL is known to us when Terraform creates it, but is hard to guess. 

At the same time, we've used a default `user` and `password` when running Terraform, and we're then switching them out via Jenkins credentials store.

The AWS CLI allows you to update environment variables, but it will only update those you provide, meaning out `SQS_QUEUE_URL` would get lost. 

This means you need the additional step of querying the function's configuration, to pick up the defaults that Terraform set, as some will be persisted.

Once you've done that, you can pull out the current variables and simply replace the ones that you don't want.

For this stage, using `jq` and piping to a temporary file is also necessary.

<script src="https://gist.github.com/the-frey/6e3ca1fdc793d4839fcfddc13d69c87d.js"></script>
