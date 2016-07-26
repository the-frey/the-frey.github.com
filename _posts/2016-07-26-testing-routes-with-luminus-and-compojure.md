---
layout: post
title: "Testing Routes with Luminus and Compojure"
description: "Testing this stuff isn't so well documented, so..."
category: 
tags: [clojure,luminus,testing]
---
{% include JB/setup %}

I've recently been trying to integration test an ElasticSearch-based web stack in clojure and had a bit of trouble working out how to make my life easier by requesting strings and getting Luminus/Compojure/Liberator to behave exactly as in a browser request cycle. This is pretty basic stuff, I'm sure, but searching for a quick explanation of which methods to use turned up zilch. In particular, testing GET requests with query params against Compojure/Liberator didn't really seem to be covered other than as an assumption that you would already know how to do it. It's official, I'm boooooooooring!

TL;DR - skip to the example at the bottom for the code.

Let's assume in a namespace like `your-app.web.routes.search` you have:

```
(defroutes search-routes
  (GET "/search" [] search-handler))
```

Now we have a standard `/search` route, as per the app example above. It's relatively trivial to come up with a handler that will pass information to your application to test the route and give you back a response:

```
(defn test-request [resource web-app & params]
  (web-app {:request-method :get :uri resource :params params}))
```

In the example above, we might:

```
(deftest search-result
  (testing "That a result page can be returned from ES"
    (is (= 200
           (:status (test-request "/search" search-routes))))))
```

However, you're quickly going to end up coming to grief on using things like query params, which if you're say, accessing ElasticSearch via a GET request, you're likely to have. In the above example, calling the route with a URL like `/search?postcode=M11EZ` will cause a `nil` to be returned. Sad times.

Let's say, as in the example above, that we want to pass a postcode (Zip code, if you prefer). Assuming we have something to unpack the query string and call ElasticSearch using that clause in our `search-handler` method above, we should be able to query.

For sanity, I'm going to represent postcodes without the customary space in the middle. Sorry, pedants.

So you're probably thinking 'there must be something built in that can handle this?' Yes, straw-man conversationalist, there is.

Enter `ring.mock.request`; require it in your test file:

```
(ns your-app.test.web.routes.search
  (require [clojure.test :refer :all]
           [ring.mock.request :refer :all]
           [your-app.web.routes.search :refer :all]))
```

Now, we can use this to actually slot neatly in to our app's routing. Our first example becomes:

```
(deftest search-result
  (testing "That a result page can be returned from ES"
    (is (= 200
        (:status (search-routes (request :get "/search")))))))
```

Then, a more complicated example with a query string (that would not have worked before):

```
(deftest search-result-with-query-string
  (testing "That a result page can be returned from ES via query string"
    (is (= 200
        (:status (search-routes (request :get "/search?postcode=M11EZ")))))))
```

Boom. I'm sure this is documented somewhere, but it took me a while to work out, so thought it was worth leaving my self-imposed blogging wilderness to document. Hopefully somebody finds it useful!

Finally, if all that default code formatting is stressing you out, here's a gist of the final example:

<script src="https://gist.github.com/the-frey/f11583c388f8875b9f04775abcfa9538.js"></script>


