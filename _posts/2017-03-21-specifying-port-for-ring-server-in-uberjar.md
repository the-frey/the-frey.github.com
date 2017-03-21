---
layout: post
title: "Specifying a port for Ring server in an Uberjar"
description: "A quick how-to"
category: 
tags: [clojure, ring, uberjar]
---
{% include JB/setup %}

Okay, so this might be blindingly obvious to everyone else, but having made a simple API using Swagger and the awesome [Compojure API library](https://github.com/metosin/compojure-api) I needed to package up a `.jar` for deployment.

Pretty simple, as it turns out -

    lein do clean, ring uberjar

However, you're quickly faced with the question of how you specify a port to run with your packaged uberjar. `env PORT=<port> java -jar /path/to/jar.jar` is ideally how we'd run our uberjar, but the question is how to achieve that. Most tutorials and docs helpfully stop at exactly this point, assuming you're happy for your ring app to run on `port 3000` but that's - I suppose - probably because there isn't a huge amount to it.

We're going to use environ to pick up variables from the environment, so add

    [environ "1.1.0"]

and an explicit version of ring 

    [ring "1.5.1"]

to your `project.clj`. It should look something like this:

<script src="https://gist.github.com/the-frey/276f88453c576a5b21a96764e47d4f13.js"></script>

Then, head into your handler namespace (for the purposes of this simple example, let's assume all your code is in the handler namespace) and add a `-main` function. This main function will be a simple call to `run-jetty` with a port specified.

<script src="https://gist.github.com/the-frey/f31140a282dec33ba5064ad11f720683.js"></script>

Obviously this is a quick canned example and you'd want to make things a bit more generic; there's quite a bit of misdirection. However, the main thing is to simply add the two new dependencies at the top of the file and the run command at the bottom.

After this, you can jar up your project and run it with:

    env PORT=<port> java -jar /path/to/jar.jar

That's it. Granted, it's a simple example - for more complex handling of environment and handling development use cases et cetera, you'll probably have to write a fair bit more code, but the essentials will stay the same. Also recall that environ plays nice with a `profiles.clj` file in development.

Note also that as this is a Swagger api, you can hit up `localhost:<port>` to see your api route and try the mock. Pretty neat.