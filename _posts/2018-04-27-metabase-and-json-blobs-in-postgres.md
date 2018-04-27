---
layout: post
title: "Metabase and JSON blobs in Postgres"
description: ""
category: 
tags: [metabase,sql,json]
---
{% include JB/setup %}

I've been using metabase to rapidly prototype some business reporting at work, and recently had to do some faffing around with an audit log that contained JSON blobs. Initially I went down the path of dumping the data through a little Clojure ETL pipeline and out into an embedded SQLite database, but we needed timestamps, so it was time to work out how to get the JSON into shape.

Turns out the native query window, which allows you to save question output, can support querying of JSON blobs, [as described on this thread and solved by user conanbatt](https://github.com/metabase/metabase/issues/708).

This is nice, as for a simple use-case it means we don't have to do any ad-hoc ETL or spin up any more services.

So, let's assume that we have a JSON blob in the field `json_field` - we can unpack it as per the following example to grab the values of the hashmap keys `"location"`, `"type"`, and `"date"`.

From the top level of the row of data, all we're interested in is the `id` field and the `event_created_at` timestamp field.

<script src="https://gist.github.com/the-frey/c2231cb2d2f863038bdb34e70a79e7cb.js"></script>

Put that in the `New Question -> Native Query` window of Metabase, and then click 'Save' to store the question. If you need results from the whole table, remove the `LIMIT` clause in the query above.
