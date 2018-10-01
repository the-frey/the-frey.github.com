---
layout: post
title: "A GraphQL API using Azure Functions and Apollo"
description: ""
category: 
tags: [azure,functions,apollo,graphql]
---
{% include JB/setup %}

I've recently been working on a small proof-of-concept in Azure. If you're looking to do a mini-API or an integration between two services that speak HTTP, then either Swagger or GraphQL is a pretty good bet.

The great thing about GraphQL for rapid prototyping is that you get, along with your data schema, a tool for exposing this via a query interface - GraphiQL.

I wanted to leverage this quick interation ability, so figured it could be the edge over Swagger/OpenAPI.

Apollo GraphQL is another useful tool for writing NodeJS GraphQL APIs, and with a bit of hacking, it's possible to get a nice mini-workflow going.

The basic requirements of this are:

1. Deployable API on Azure
2. Scalable
3. Can't take too much devops time
4. Must be able to run locally

I decided GraphQL was a good fit, and so looked to get it set up in the mix. This added the additional complication:

5. GraphiQL endpoint should not be exposed when deployed

So this meant building out an Azure function as a graphql query endpoint, then making the schema backing that function accessible, so that a small Express web server could read that and use it to run a GraphiQL endpoint.

I found Apollo helped with getting this out of the door fast.

FWIW I also found that the Azure Functions local tools are actually pretty damn good. For cross-platform goodness I'm actually building the project via Serverless and just using Azure for local dev, though I've taken the Serverless config out of the example repo as it was a bit noisy.

```
    brew tap azure/functions
    brew install azure-functions-core-tools

    # to create a fn
    func init # add -n flag to prevent git init
    func new

    # to run a fn
    func start
```

Here's a link to the example repo. At the moment it's just a simple address lookup with an additional DateTime type. Extending and adding new types with input and output serialisation is made easier with Apollo - it's another of the nice features.

[GraphQL Example Repo](https://github.com/the-frey/graphql-azure-example)

- [Main function](https://github.com/the-frey/graphql-azure-example/blob/master/graphqlEndpoint/index.js)
- [Schema](https://github.com/the-frey/graphql-azure-example/blob/master/graphqlEndpoint/schema.js)
- [Resolvers](https://github.com/the-frey/graphql-azure-example/blob/master/graphqlEndpoint/resolvers.js)
- [GraphiQL server](https://github.com/the-frey/graphql-azure-example/blob/master/utils/graphiql.js)

The Azure function will hot-reload, but if you change your schemas, you'll need to kill the Express server and restart GraphiQL.

To run:

```
    npm install

    func start

    # new terminal tab
    npm run graphiql
```

Graphiql will then be able to connect to your function. Hit `localhost:3000/graphql` to see the query editor, and use the 'copy as cURL' command to get something you can use in the terminal to hit your running function outside of the editor.

Deploying this is outside of the scope of this quick blog, but look in the Azure docs for how to do that.


