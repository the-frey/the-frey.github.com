---
layout: post
title: "Entering a Running Docker Container"
description: "Or, 'I wish I had found this out three weeks ago...'"
category: 
tags: [devops, docker, development]
---
{% include JB/setup %}

So for the last few weeks, I've been working with Docker to simplify some of our devops processes. Docker itself is really cool, and it's now production-ready (an assertion, incidentally, that I _do_ agree with). I gave a talk at North West Ruby User Group on what I've found out while using it as part of a non-trivial system [more info here](http://nwrug.org/events/thursday-17th-july---a-practical-introduction-to-docker/), and hopefully I will post some slides at some point or a more detailed blog with some 'getting started with Rails'-type spiel. 

However, the single, most useful thing I've found is this little tool, by one of the Docker core developers. You may already be able to leverage `nsenter`, but without writing some bash scripts of your own, you probably won't get the simplicity of `docker-enter` without quite a lot of fiddling. One of the things I've most often wanted to do was attach to a Docker Container and start additional processes. When debugging, I've spent far too much time spinning up containers and mounting logs or starting new containers with just a shell because there's not really an official way of doing this (Docker Containers, rightly, should do only one thing, and do it well, after all) if the container is already running - you've got to start over. So yeah, save yourself a shitton of time and a bunch of head-desking while reading forums, mailing lists and GitHub issue trackers and just install this wherever you are planning on using docker: [nsenter package with docker-enter wrapper](https://github.com/jpetazzo/nsenter). Scroll down in the readme for the mention of `docker-enter`, and thank me later. 