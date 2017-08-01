---
layout: post
title: "Networking between docker containers with docker-host"
description: ""
category: 
tags: [docker,]
---
{% include JB/setup %}

From within a Docker container, from time to time you'll need to access other containers on the same box. Often this can be achieved by the standard Docker environment variables, but as these are set when the container starts up, these require the use of `sed` or similar to swap out hardcoded values for the dynamically generated location of the containers you're working with. 

It can therefore be useful to just have a `docker-host` entry to refer to the parent box in `/etc/hosts` that can then be passed into all containers started on that box. In this way, one could refer for example to memcached via `docker-host:11211` when say configuring the Dalli client in Ruby, or Elasticsearch like so, ` Elasticsearch::Model.client = Elasticsearch::Client.new(host: "docker-host:9200")` or DB endpoint for a DB like Stardog which you call over HTTP as simply `http://docker-host:5820/database-name/query`.

On the host, in a bash script you can include the following line to add the entry to the hosts file:

    echo "`/sbin/ifconfig ens4 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'` docker-host" | sudo tee -a /etc/hosts

Or, where docker is started simply use the `--add-host` run option to ensure that is available to services inside the container.

    docker run -p 127.0.0.1:8080:8080 \
      --env-file=/path/to/env.list \
      --add-host docker-host:`/sbin/ifconfig ens4 | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'` \
      --name container-name \
      you/your_project:build_xxx