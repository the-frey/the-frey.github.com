---
layout: post
title: "Monitoring Docker Containers with Monit"
description: "Docker only became production-ready officially a few months ago, but monitoring is a big issue. Here's what we're doing at Swirrl."
category: 
tags: [devops, docker, monit]
---
{% include JB/setup %}

Over the past month or so at [Swirrl](http://www.swirrl.com) I've been working on using Docker to sandbox all of our microservices (there can be as many as twelve in a full deployment setup). In getting these production ready, one of the biggest issues I've run into is monitoring. It took some time to work out, and it's by no means perfect, but here's a potential solution if you want a simple fix. 

At first I looked at Supervisor, but for a variety of reasons (having to foreground processes being one of the main ones) decided against it. Also, we already use monit in production, so any big fuckups and I could badger Ric to fix them. Win. 

So, I needed to set up two different pieces of monitoring: 

1. Check each host on the physical server
2. Check each container is running

For each monit cycle (I set it to 60 seconds, but YMMV), checking each host was trivial and worked right away - the monit [docs](https://mmonit.com/monit/documentation/) are actually pretty good, and for the simple cases there's also ample examples in the `monitrc` file that ships with the install.

The problem of course with monit is that you need Pidfiles in order to run up and monitor services using the standard process hooks. Time for a wrapper script:

<script src="https://gist.github.com/the-frey/742e47bcefc0258ee414.js"></script>

Which can be called by the monit process manager: 

<script src="https://gist.github.com/the-frey/466a53a6d1fc926dbd4f.js"></script>

Except that this won't work well. There's a lot of reasons - and if you're thinking CLI arguments within the monit DSL then yes, that's a problem I ran into as well, but also cleaning up pidfiles. There's not an exit hook for Docker containers, and jury-rigging a mixture of `docker wait`, `docker attach` and API calls was a hacky rabbithole that didn't seem likely to even work anyway. 

Luckily, I remembered that there was a script runner in monit. From the docs: 

"Monit will execute the program periodically and if the exit status of the program does not match the expected result, Monit can perform an action[...] Monit [can be configured to] raise an alert if the exit value of [program] is different from 0. By convention, 0 means the program exited normally. Program checks are asynchronous."

That last part is key; it means that monitrc will continue evaluation and that I could test multiple docker containers every sixty seconds, even with a slow bash script that could fail. It actually ended up being so simple I didn't even need to specify a `timeout` directive in the `monitrc`, but more on that in a second... 

So, all we need now is a command that returns non-zero if a container is stopped or does not exist; `docker logs` is out, as that will return 0 even for stopped containers, but wait... `docker top`, right? 

Thus, we can simplify the wrapper script: 

<script src="https://gist.github.com/the-frey/a1e87ce77e0cff72f2be.js"></script>

Hooking it up to monitrc thus:

<script src="https://gist.github.com/the-frey/3a6f826ec89d3bca3037.js"></script>

Note that because the CLI args get a bit screwy, it isn't as DRY as I would like. Obviously you can get around it by doing a second wrapper, but it's maybe more declarative to have 7 files clearly named for 7 containers instead of another layer of fiddling. Or something. 

So there you go, we've got this running in our staging environment and so far it seems to work. Both port/ping testing and monitoring running docker containers using monit. Boom. 

*If you're also monitoring Docker containers then I would love to hear how you're doing it, feel free to [tweet me](https://www.twitter.com/hipsters_unite) or find me on #docker on irc if I'm about.*
