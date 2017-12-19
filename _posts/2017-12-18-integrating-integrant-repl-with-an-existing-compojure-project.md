---
layout: post
title: "Integrating Integrant repl with an existing Compojure project"
description: "A quick follow-up"
category: 
tags: [clojure,integrant,repl]
---
{% include JB/setup %}

So in my <a href="/2017/12/14/compojure-and-integrant">Compojure and Integrant</a> post I step through some of the bare how-and-why of using Integrant.

For development, however, you're probably going to want to use the reloaded workflow, and luckily the [integrant-repl](https://github.com/weavejester/integrant-repl) library can give you most of the hooks you need.

So, from my `compojure-api` example before, we need to do the following to get it set up:

    $ mkdir -p env/dev/clj
    $ touch env/dev/clj/user.clj

You should then have a directory structure like this, which leaves a `user.clj` file on your classpath:

```
env
└── dev
    └── clj
        └── user.clj
```

Make sure this is added to your dev source paths in your `project.clj` - mine looks a little like this:

```clojure
(defproject my-compojure-api-app "0.1.0-SNAPSHOT"
   :description "FIXME: write description"
   :dependencies [[org.clojure/clojure "1.8.0"]
                  [metosin/compojure-api "1.1.11"]                  
                  [cheshire "5.5.0"]
                  [ring "1.6.3"]
                  [ring/ring-mock "0.3.0"]
                  [com.novemberain/monger "3.1.0"]
                  [environ "1.1.0"]
                  [spootnik/constance "0.5.3"]
                  [clojure-future-spec "1.9.0-beta4"]
                  [integrant "0.6.2"]
                  [org.clojure/tools.logging "0.4.0"]]
   :ring {:handler my-compojure-api-app.handler/app}
   :uberjar-name "my-compojure-api-app.jar"
   :profiles {:dev {:dependencies [[javax.servlet/javax.servlet-api "3.1.0"]
                                   [integrant/repl "0.2.0"]]
                    :plugins [[lein-ring "0.12.0"]]
                    :source-paths ["env/dev/clj"]}})
```

Let's assume that in my handler namespace `my-compojure-api-app.handler`[1] I have included my other components specified in `my-compojure-api-app.db.core`. Make sure this is included in the `user.clj` file.

So our `my-compojure-api-app.db.core`, using some of the logic suggested in the Integrant docs[2], looks like:

<script src="https://gist.github.com/the-frey/6a89abdf4ce9f5be98fb18bd47a02585.js"></script>

Our `my-compojure-api-app.handler` looks like:

<script src="https://gist.github.com/the-frey/9fd9cd73e52229c1c5d6097cd45b1dba.js"></script>

And finally our user.clj looks like:

<script src="https://gist.github.com/the-frey/cc93fe7dff405cff6c30b9d7e49f3c03.js"></script>

Now we can

```
user=> (prep) ;; to prep config
user=> (init) ;; to start app
user=> (go) ;; to do both
user=> (halt) ;; to stop
```

Huzzah!

[1] Okay, before releating this I would probably put all this logic in `my-compojure-api-app.core` and include only that, but this is a limited example I'm using.

[2] You have to make sure you don't start Jetty in the foreground or it will block and lock up your repl.
