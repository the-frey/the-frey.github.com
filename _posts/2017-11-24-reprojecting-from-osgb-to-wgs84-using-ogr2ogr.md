---
layout: post
title: "Reprojecting from OSGB to WGS84 using OGR2OGR"
description: ""
category: 
tags: [osgeo,ogr2ogr,administrative geography]
---
{% include JB/setup %}

Most UK administrative geography uses the OSGB projection, which isn't much use if you're needing WGS84, which is used for most mapping work. I'm glossing over that a bit, but look it up if map projections interest you.

The [ONS GeoPortal](http://geoportal.statistics.gov.uk/) gives you a wide range of boundaries that you might want to use for mapping purposes, but you'll need to convert them first. 

So first grab yourself a zipfile, and then convert it:

    ogr2ogr -f "ESRI Shapefile" <output-file> <file-you-just-downloaded> -t_srs EPSG:4326 -s_srs EPSG:27700

You can then use a tool like [Mapshaper](http://mapshaper.org) for polygon simplification in order for this to be performant enough for web use. To give you an idea - the input shapefile for all Output Areas in the UK is 600MB+, and with a usable degree of simplification in place can be reduced to around 25MB.

Mapshaper can also be used to convert the shapefile to geojson.

After I'd pretty much finished doing this, my friend Robin also pointed out his [gist on the subject](https://gist.github.com/Robsteranium/df5e3b42e05f446b718a), which is better than this post. So yeah. 