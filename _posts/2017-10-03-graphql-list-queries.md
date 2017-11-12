---
layout: post
title: "GraphQL list queries"
description: "A quick post to explain how this works..."
category: 
tags: [graphql, list, queries]
---
{% include JB/setup %}

When I was first looking into GraphQL I had to ask on StackOverflow to get my head around how this worked - I have always depended on the kindness of strangers.

Assuming you have the examples in the graphql-clj-starter, or indeed you've cloned [my extensions](https://github.com/the-frey/graphql-clj-starter/blob/master/src/graphql_clj_starter/graphql.clj), add a query type:

```
type Query {
  droids(ids: [String!]): [Droid!]
}
```

This will allow you to make a Query like so:

```
query Q($list: [String!]){
  droids(ids: $list){
    name
    primaryFunction
  }
}
```

With variables like so:

```
{
  "list": ["2000", "2001"]
}
```

That will return the following:

```
{
  "data": {
    "droids": [
      {
        "name": "C-3PO",
        "primaryFunction": "Protocol"
      },
      {
        "name": "R2-D2",
        "primaryFunction": "Astromech"
      }
    ]
  }
}
```

If you clone my project, run it and go to the address lein runs the server on (I think it's 3002 by default) in your browser, you can try this for yourself in GraphiQL.