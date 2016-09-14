---
layout: post
title: "Composable Liberator Resources"
description: "Using Liberator maps in practice"
category: 
tags: [clojure, luminus, liberator]
---
{% include JB/setup %}

I touched on this in my [last post](https://the-frey.github.io/2016/07/29/more-sane-liberator-resources-with-multiple-content-types), but I thought it might be useful to provide another example. Here's where we were up to last time.

I've slightly changed `hello-world-map` to accept JSON, for reasons we'll see later. We've also changed the route for that map to `/hello`:

<script src="https://gist.github.com/the-frey/2462fbcad44b86343a0ac4de3e7e431c.js"></script>

NB: other dependencies are required here, and I've not included them all, just the liberator/ring specific ones.

So let's say we want to implement an API, and we've got users with API keys that will be passed along with a request. Now, this isn't super-hard to do in a framework like Rails, but I guarantee you that it can't be done as elegantly as in the simple Clojure version that follows. 

First, let's use the ring `basic-authentication-request` function to help us build a token auth fn. 

`basic-authentication-request` takes a request and a function that checks whether the user is authorized. The API key will need to be passed base64-encoded in an `authorization` header.

<script src="https://gist.github.com/the-frey/f63cc3afad17f39d81994fdff518bd49.js"></script>

It returns a key, `:basic-authentication` if successful, as can be seen in the [source](https://github.com/remvee/ring-basic-authentication/blob/master/src/ring/middleware/basic_authentication.clj#L34):

```
(defn basic-authentication-request
  "Authenticates the given request against using auth-fn. The value
  returned by auth-fn is assoc'd onto the request as
  :basic-authentication.  Thus, a truthy value of
  :basic-authentication on the returned request indicates successful
  authentication, and a false or nil value indicates authentication
  failure."
  [request auth-fn]
  (let [auth ((:headers request) "authorization")
        cred (and auth (decode-base64 (last (re-find #"^Basic (.*)$" auth))))
        [user pass] (and cred (s/split (str cred) #":" 2))]
    (assoc request :basic-authentication (and cred (auth-fn (str user) (str pass))))))

```

The auth-fn we are using accepts an email and token and checks whether the user contains a `:token` or `:api-key` field that matches (this is a [monger](http://clojuremongodb.info/) resource, in case you're curious). The idea here is that you could say use one for a form field login, and the other for the API.

Note that it's good practice to include a constant time checker (here represented by `===`) to avoid timing attacks.

<script src="https://gist.github.com/the-frey/984004340228575c875503b55b6240d0.js"></script>

Now, we're ready to write a Liberator resource map that we can then call from a compojure route.

Let's say that our `hello-world-map` from the previous example is in a `ns` called `hello` under `routes`.

<script src="https://gist.github.com/the-frey/088f929c612f63385247234259188abd.js"></script>

Whoa! That wasn't very painful, was it? So now if you request `/hello` (maybe there's session-based auth going on in a middleware elsewhere) you don't expressly need a base64 encoded `authorization` header, as you now do with `/api/hello`.

You could also completely refactor the original map to handle auth within the Liberator map, using a similar or different pattern. An alternative use would be a `def` called `map-defaults` with common settings for all routes, which would be merged with other functionality like our `:authorized?` example above.

Either way, ignoring the somewhat contrived example, it's a cool and useful pattern that I hope I've demonstrated.
