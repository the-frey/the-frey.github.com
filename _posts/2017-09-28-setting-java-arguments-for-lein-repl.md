---
layout: post
title: "Setting java arguments for lein repl"
description: "Just a quick tip"
category: 
tags: [java,clojure,leiningen]
---
{% include JB/setup %}

If, like me you often have to run things like data tasks that are an absolute dog in terms of resource usage, you'll want to change your memory settings for java. Normally you'd put these in your shell profile, but if you don't want them to persist, then it's as simple as you'd expect to get it working for your repl. It may not be quite as ephemeral as inline variables when running an uberjar, but it's not far off.

    export _JAVA_OPTIONS="-Xms1024m -Xmx2G -Xss256m"
    lein repl

    Picked up _JAVA_OPTIONS: -Xms1024m -Xmx8G -Xss256m
    nREPL server started on port 60408 on host 127.0.0.1 - nrepl://127.0.0.1:60408
    REPL-y 0.3.7, nREPL 0.2.12
    Clojure 1.8.0
    Java HotSpot(TM) 64-Bit Server VM 1.8.0_144-b01
        Docs: (doc function-name-here)
              (find-doc "part-of-name-here")
      Source: (source function-name-here)
     Javadoc: (javadoc java-object-or-class-here)
        Exit: Control+D or (exit) or (quit)
     Results: Stored in vars *1, *2, *3, an exception in *e

    energy-pipeline.pipeline=> 

That might be totally obvious to some, but it only occurred to me today as a solution to allow me to develop a bit more effectively with my emacs/lein/cider combination and large input files. Anyway...