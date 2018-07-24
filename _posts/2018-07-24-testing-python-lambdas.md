---
layout: post
title: "Testing Python Lambdas"
description: ""
category: 
tags: [python,lambda,serverless]
---
{% include JB/setup %}

This is a continuation of my [post on structuring a serverless Python project](https://the-frey.github.io/2018/07/20/structuring-a-serverless-python-backend). I got asked a couple of times what a producer would look like, and how exactly tests would work, so I put together this basic example.

So - given the project structure from the previous post, we can fill in the producer that logs our events:

<script src="https://gist.github.com/the-frey/00e30cd6664327d88fab2caa7b3fdc2e.js"></script>

As this doesn't need native extensions, you can paste it into the console without using the Docker build step. It's also worth looking at how you manage your partitions if you intend to publish a lot of events, but I'll just [direct you to the docs](https://docs.aws.amazon.com/streams/latest/dev/key-concepts.html), as that's out of scope of this post.

This producer, as it stands is meant to simply push the following JSON into Kinesis; you can add this as the test event via the UI:

```
{
  "id": "7",
  "path": "/login", 
  "status": "200", 
  "event_created_at": "2018-07-20T13:28:59.529Z"
}
```

If you're sharp, you've spotted that if this Lambda was an HTTP endpoint, then the event this Lambda would process would look very different. We're just using this as a simplified representation.

[On this page](https://docs.aws.amazon.com/lambda/latest/dg/eventsources.html) in the Amazon docs, they have example events for all the event sources you could need - it's what we used to reference our Kinesis events elsewhere.

Anyway, the key thing here is that as we've broken out our transform function - which currently just encodes the payload, when we need to refactor it to actually transform the input (below) into the fields we care about (above), we can refactor that within the function, and unit test it.

So, if we take the event JSON above for now, put it in a fixtures file at `./test/fixtures/json_event_fixture.json`, then we can unit test the transformation cleanly - even if it doesn't do much for now.

<script src="https://gist.github.com/the-frey/b9936714039ebcdef599f2338b616b16.js"></script>

Finally, as promised, here's the mega payload you get from the API gateway request - you'll see that our transform is going to have to get a bit more complex to massage it into the right shape!

Final note - if your endpoint is RESTful, referring to a resource type, this is a really great opportunity to reify a canonical representation for an entity type, e.g. User. I have some more thoughts around the opportunities presented by streaming and data schema reification in [my post on Kafka and enterprise knowledge graphs](https://the-frey.github.io/2018/04/25/kafka-and-the-enterprise-knowledge-graph).

Anyway, here's the structure:

```
{
  "path": "/test/hello",
  "headers": {
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8",
    "Accept-Encoding": "gzip, deflate, lzma, sdch, br",
    "Accept-Language": "en-US,en;q=0.8",
    "CloudFront-Forwarded-Proto": "https",
    "CloudFront-Is-Desktop-Viewer": "true",
    "CloudFront-Is-Mobile-Viewer": "false",
    "CloudFront-Is-SmartTV-Viewer": "false",
    "CloudFront-Is-Tablet-Viewer": "false",
    "CloudFront-Viewer-Country": "US",
    "Host": "wt6mne2s9k.execute-api.us-west-2.amazonaws.com",
    "Upgrade-Insecure-Requests": "1",
    "User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.82 Safari/537.36 OPR/39.0.2256.48",
    "Via": "1.1 fb7cca60f0ecd82ce07790c9c5eef16c.cloudfront.net (CloudFront)",
    "X-Amz-Cf-Id": "nBsWBOrSHMgnaROZJK1wGCZ9PcRcSpq_oSXZNQwQ10OTZL4cimZo3g==",
    "X-Forwarded-For": "192.168.100.1, 192.168.1.1",
    "X-Forwarded-Port": "443",
    "X-Forwarded-Proto": "https"
  },
  "pathParameters": {
    "proxy": "hello"
  },
  "requestContext": {
    "accountId": "123456789012",
    "resourceId": "us4z18",
    "stage": "test",
    "requestId": "41b45ea3-70b5-11e6-b7bd-69b5aaebc7d9",
    "identity": {
      "cognitoIdentityPoolId": "",
      "accountId": "",
      "cognitoIdentityId": "",
      "caller": "",
      "apiKey": "",
      "sourceIp": "192.168.100.1",
      "cognitoAuthenticationType": "",
      "cognitoAuthenticationProvider": "",
      "userArn": "",
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.82 Safari/537.36 OPR/39.0.2256.48",
      "user": ""
    },
    "resourcePath": "/{proxy+}",
    "httpMethod": "GET",
    "apiId": "wt6mne2s9k"
  },
  "resource": "/{proxy+}",
  "httpMethod": "GET",
  "queryStringParameters": {
    "name": "me"
  },
  "stageVariables": {
    "stageVarName": "stageVarValue"
  }
}
```

This is probably subject to some change, so be sure to inspect the docs via the link above.