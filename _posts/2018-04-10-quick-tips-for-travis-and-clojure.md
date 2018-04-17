---
layout: post
title: "Quick tips for Travis and Clojure"
description: "Writing this down before I forget it for good"
category: 
tags: [travis,clojure,ci]
---
{% include JB/setup %}

So, assuming you're building using lein, you might want to add a profiles file but be able to override it. This means dumping some config into `/etc/leiningen/profiles.clj`. To do this, in a basic form, you can do the following in a new project:

    $ mkdir travis
    $ touch travis/bootstrap.sh
    $ touch travis/profiles.clj

Your folder should look like:

    travis
    ├── bootstrap.sh
    └── profiles.clj

Then:

    $ echo "{}" > ./travis/profiles.clj

Change the shell script `bootstrap.sh`:

```
#!/bin/bash

sudo chown -R travis ./travis/*
sudo mkdir -p /etc/leiningen/
sudo mv ./travis/profiles.clj /etc/leiningen/profiles.clj
```

Then, in your `.travis.yml`:

```
language: clojure
sudo: required
lein: 2.7.1
jdk:
  - openjdk7
  - oraclejdk8
before_install:
  - sudo chmod +x ./travis/bootstrap.sh
  - sudo ./travis/setup-lein-travis.sh

script: travis_retry env JAVA_HOME=/usr/lib/jvm/java-8-oracle/bin/java lein test
```

We use `sudo: required` to force Travis to not run in a container. I'm paranoid and I've had issues with resource usage in the past. The explicit setting of `JAVA_HOME` is another piece of paranoia from when the Java version inside Travis was repeatedly getting reset by a database dependency that hard-coded it in a start script. I like to be certain now.

I tend to use `travis_retry` as a default option for tests unless I'm certain that I want them to crash out.

This can be useful in e.g. a Ruby/Rails or Django stack with complicated testing suites. By tagging and separating out your unit and integration tests, you can run your unit tests without the retry option, but run for example UI tests using the retry. In Ruby/RSpec land, you can also use `--fail-fast` to mean that the overall run time is reduced in the pathological failure case.
