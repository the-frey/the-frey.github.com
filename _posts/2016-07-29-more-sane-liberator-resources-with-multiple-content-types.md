---
layout: post
title: "More sane Liberator Resources with multiple content types"
description: "I've spent some time getting familiar with Liberator, so here's some thoughts..."
category: 
tags: [clojure,liberator,compojure]
---
{% include JB/setup %}

TL;DR - **Liberator resources are constructed from hashmaps, so an alternative to using `defresource` macros is to assemble your resources by merging composable hashmaps together at the latest possible moment. This also makes testing easier to reason about.**

Okay, so recently I've been working with [Liberator](https://clojure-liberator.github.io/liberator) (in the context of Luminus's default structure) to handle HTTP and content negotiation for the application. There's a couple of patterns for creating resources - `resource` and the `defresource` macro, and I'm going to step through some examples from the docs while showing the pattern that has ended up working best for us so far.

Big ups of course go to our resident Clojure guru Rick for pairing on stuff and fielding dumb questions!

Right, let's get started. From the docs, here's a simple `defresource`:

```
(defresource hello-world
  :available-media-types ["text/plain"]
  :handle-ok "Hello, world!")
```

In practice though, as you're going to want to set defaults for the large number of keys available to you, you'll end up defining a `defaults` method - `defresource` just takes a hashmap as its second argument, so we can do something like this, hooking it up to a compojure route in the process:

```
(def hello-world-defaults
  {:available-media-types ["text/plain" "text/csv"]})

(defresource hello-world hello-world-defaults
  :handle-ok "Hello, world!")

(defroutes my-app
  (ANY "/" [] hello-world)) ;; we use ANY as Liberator should handle request type
```

Okay, so let's say we have a second resource that takes the same defaults but has an extra key. Let's create a new `defresource`:

```
(def hello-world-defaults
  {:available-media-types ["text/plain" "text/csv"]})

(defresource hello-world hello-world-defaults
  :handle-ok "Hello, world!")

;; we're getting a resource with an :id, so it may not exist
;; :exists? allows you to return a hash to be added to ctx - so we are adding in, say some extra info about the user
(defresource user-resource hello-world-defaults
  :exists? (fn [ctx]
              (let [user-id (:id (:params (:request ctx)))
                    associated-users (get-associated-users user-id)] ;; let's say this function is defined elsewhere
              {:associated-users associated-users}))
  :handle-ok (fn [ctx]
              (render-user-resource ctx))) ;; let's say we have a render function elsewhere

(defroutes my-app
  (ANY "/" [] hello-world)
  (ANY "/user/:id" [] user-resource))
```

So, here's where it becomes a matter of taste. If we want to replicate this behaviour in tests, we've got an issue - really we need to recreate the `defresource` to get access to that extra `:exists?` key. 

"Ah!" you might say, "but you could just move that into a map and include that into the `defresource` in the same way!" Well, yes, that's the next step:

```
(def hello-world-defaults
  {:available-media-types ["text/plain" "text/csv"]})

(def user-resource-defaults
  (merge {:exists? (fn [ctx]
                      (let [user-id (:id (:params (:request ctx)))
                            associated-users (get-associated-users user-id)] ;; let's say this function is defined elsewhere
                        {:associated-users associated-users}))
          :handle-ok (fn [ctx]
                        (render-user-resource ctx))}
         hello-world-defaults))

(defresource hello-world hello-world-defaults
  :handle-ok "Hello, world!")

;; we're getting a resource with an :id, so it may not exist
(defresource user-resource user-resource-defaults)

(defroutes my-app
  (ANY "/" [] hello-world)
  (ANY "/user/:id" [] user-resource))
```

Except that now, we don't really need the named `defresource` do we? 

```
(def hello-world-map
  {:available-media-types ["text/plain" "text/csv"]})

(def user-resource-map
  (merge {:exists? (fn [ctx]
                      (let [user-id (:id (:params (:request ctx)))
                            associated-users (get-associated-users user-id)] ;; let's say this function is defined elsewhere
                        {:associated-users associated-users}))
          :handle-ok (fn [ctx]
                        (render-user-resource ctx))}
         hello-world-defaults))

(defresource hello-world hello-world-defaults
  :handle-ok "Hello, world!")

;; we're getting a resource with an :id, so it may not exist
(defresource user-resource user-resource-defaults)

(defroutes my-app
  (ANY "/" [] (resource user-resource-map))
  (ANY "/user/:id" [] (resource (merge hello-world-map
                                       {:handle-ok "Hello, world!"}))))
```

This also has the added bonus that we can use the following incantation to create a working resource in tests, merging in maps of other options that we may need:

```
(resource (merge user-resource-map
                 user-resource-test-map)) ;; maybe this adds a new key we don't need in dev/prod 
```

We could create a new resource from scratch that dispatches on media type to two different handlers:

```
(def test-resource-options
  {:handle-ok (fn [ctx]
                (log/info ctx)

                  (let [media-type (get-in ctx [:representation :media-type])
                        request (:request ctx)]
                    (if (= "text/html" media-type)
                           (render-resource-page)
                           (str "Media type was" media-type)))})

(resource (merge hello-world-map
                 test-resource-options))
```

That's a bit of a contrived example; maybe you want to be able to handle different resources (in this case, HTML and CSV), but you've not written a proper CSV handler yet and just want to sanity check. Bad example, but you get the idea.

Obviously, there are drawbacks to using this method, for one - it works mainly as we can (in this example) rely entirely on the `ctx`, the context provided by Liberator, without needing the original request verbatim - but for now, we've found that the pattern of assembling hashmaps to represent resources rather than using the macro to create named resources (or, indeed, even `def`ing the resources we've created) works well.

