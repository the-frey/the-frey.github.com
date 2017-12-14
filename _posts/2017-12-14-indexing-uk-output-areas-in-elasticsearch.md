---
layout: post
title: "Indexing UK Output Areas in Elasticsearch"
description: "Argh"
category: 
tags: [elasticsearch,geojson,data]
---
{% include JB/setup %}

Recently I had to hack together a quick setup to index a bunch of Output Areas in between doing other stuff, so I put in a few commits to make something I could extend in future... though it's still pretty hacky for now.

Using the steps outlined in my simplifying and reprojecting post, I changed the projection of the OAs and mashed them into ES, although some throw an error - the unsimplified, unreprojected OAs that error for me are in the repo as GeoJSON. At some point I'll replace them with the broken-up OAs from the data I've created that correspond with them (by GSS code) and then run prepair on them.

Either way, the code and instructions for UK OAs are in the [GeoJSON Shipbreaker repo](https://github.com/the-frey/geojson-shipbreaker).