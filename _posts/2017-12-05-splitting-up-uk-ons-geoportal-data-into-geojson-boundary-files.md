---
layout: post
title: "Splitting up UK ONS Geoportal Data into GeoJSON Boundary Files"
description: "A quick snippet for churning this out..."
category: 
tags: [clojure,etl,geojson]
---
{% include JB/setup %}

As you may know if you've ever used the [UK Geoportal site](http://geoportal.statistics.gov.uk/) to get boundaries as features or GeoJSON, once you've converted and re-projected your files you probably still have a file that's too large to serve over the wire, even if you've simplified them with a tool like [mapshaper](http://mapshaper.org).

A common approach to get around this is to split the file up into GeoJSON features, with a single feature per file, or load the whole lot into ElasticSearch. I might write a quick blog at some point about the Elasticsearch ETL process, but here's a quick snippet to split out a file, in this case the Output Areas file, into thousands of GeoJSON files named by their GSS code.

<script src="https://gist.github.com/the-frey/42f3ca4a3157d3013f84e0674bc698c7.js"></script>
