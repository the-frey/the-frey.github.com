---
layout: post
title: "Working With Stardog's HTTP API"
description: "I've spent a couple of weeks playing around with Stardog and PMD, so here's some useful quick start things I've learned."
category: 
tags: [pmd, stardog, database]
---
{% include JB/setup %}

I get the feeling that not many people use Stardog's HTTP API, instead opting to use the CLI interface, but in case you intend to (with or without the Swirrl PMD application), you might find the following guide of some use.

Stardog is a graph data store built on top of Sesame. I didn't mention that, but y'know, if you're not into graph data then probably the rest of this post won't be of much interest.

## Setup

Though the Stardog [docs](http://docs.stardog.com/) suggest this is not the case, you will need to `cd` into the `$STARDOG_HOME` directory setup during their quick launch guide, then into the `bin` directory. Once there, `./stardog` and `./stardog-admin` should work. 

To start and stop the Stardog server, add the following commands to your `.zshrc` or equivalent - these assume that you've installed Stardog in a folder on the User root: 

    alias walkies="cd $HOME/stardog-2.1.3/bin ; ./stardog-admin server start ; cd $HOME"
    alias hometime="cd $HOME/stardog-2.1.3/bin ; ./stardog-admin server stop ; cd $HOME"

Creating a DB is simple. 

    ./stardog-admin db create -n database-name input.ttl another_file.rdf
    
Dropping a database can be done like so:
    
    ./stardog-admin db drop database-name

In the `development.rb` file for your PMD app (if you're not using Publish My Data, then ignore this bit), you will need to point the SPARQL endpoint at Stardog: 

    config.sparql_endpoint = 'http://localhost:5820/your-database-name-here/query'

Note that the query endpoint is different from Fuseki's `/sparql` default.

To make queries run by default against all named graphs, some configuration is needed. As Stardog says: 

    for answering queries that don't specify a dataset (FROM or FROM NAMED), the SPARQL specification says that the query should be answered only using the default graph (no named graphs)
    
Setting `query.all.graphs = true` in `stardog.properties` (see Setting up Logging below for more info on this file) will answer these queries using all the data in the database, "including the default graph and all named graphs."

If there are problems with the CLI commands, the documentation isn't as comprehensive as that which can be found by using

    ./stardog help
    ./stardog-admin help

The `help` command can be used in conjunction with any other to illuminate usage. Note that Stardog 2.1.3 provides a default user “admin” and password “admin” in `stardog-admin` commands if no user or password are given. This is _not_ the case with the HTTP API, where credentials are required.

## Disabling Credentials

By default, a user and password need to be passed to Stardog in order for commands and queries to work. From the CLI, `admin:admin` is passed if nothing is specified; all the examples below assume that creds are still required and the default user is still the Stardog default of admin with password admin. If, however, you want to not worry about providing credentials, then pass the `--disable-security` flag to `stardog-admin` when you start the Stardog server. 

## Setting up Logging

In the `stardog.properties` file (located at `$STARDOG_HOME/examples/stardog.properties`) there are flags for enabling access and audit logging; the latter is a superset of the former. `cp` this file into the Stardog installation directory with the desired settings and you should have a log (either `access.log` or `audit.log`) available to `tail -f`.

## Adding Data to a Named Graph

Note, when issuing a request via `curl` to the Stardog HTTP API you will need to supply credentials if they are set. The default is 'admin' with password of 'admin' or 'anonymous', with password 'anonymous'; with a GET you can pass these in the form `-u admin:admin` as a command-line option, though when issuing a POST you will need to pass them in an `Authorization` header. The values are encoded in the RFC2045-MIME variant of Base64, except not limited to 76 char/line. That means that `YWRtaW46YWRtaW4=` is the result of encoding 'admin:admin'.

1. First, begin a transaction by curling the database (assuming your database name is "database-development"):

    curl -v -X POST -H "Authorization: Basic YWRtaW46YWRtaW4=" -H "text/plain" "http://localhost:5820/database-development/transaction/begin"

2. Take the token given to you (in this case, 'c34c3c2c-4307-480a-b5d0-9484938c0028') by the transaction and use it to make the action:

    curl -v -X POST -d @themes.ttl -H "Authorization: Basic YWRtaW46YWRtaW4=" -H "Content-Type: text/turtle" "http://localhost:5820/database-development/c34c3c2c-4307-480a-b5d0-9484938c0028/add?graph-uri=http://linked.glasgow.gov.uk/graph/concept-scheme/themes" 

3. Commit the transaction: 

    curl -v -X POST -H "Authorization: Basic YWRtaW46YWRtaW4=" -H "text/plain" "http://localhost:5820/database-development/transaction/commit/c34c3c2c-4307-480a-b5d0-9484938c0028"

## Known Issues as of v2.1.3

1. When querying with a `DESCRIBE` after setting the all named graphs flag, no triples are returned - use a `CONSTRUCT` instead. This _will_ be fixed in later versions. 
2. When uploading files of ~ >1MB to Stardog via the HTTP API you may find that they error with a status of 500. The `SD-Error-Code` specific headers [more info](http://docs.stardog.com/http/#sd-SPARQL-Protocol) should provide you with more info in case of error.

Most of this can, of course, be found in the Stardog docs, but it took me a while and a fair bit of digging to get cracking with the HTTP API and curl commands, so maybe this will be of some use to somebody. 
