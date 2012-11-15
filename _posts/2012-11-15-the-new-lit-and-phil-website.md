---
layout: post
title: "The new Lit and Phil website"
description: ""
category: 
tags: [accessibility, wordpress, php, strategy]
---
{% include JB/setup %}

Working on the new Lit &amp; Phil website has been a really interesting experience, mainly because it really does hammer home some points about accessibility. Sure, you're taught about it, and sure, you know that it's important for SEO, but things change a little when you're actually faced with the probability that a good portion of your users will be either:

a) Not especially IT Literate 

b) Possessed of a minor accessibility issue (e.g. bad eyesight or motor function). 

To increase usability for stakeholders, officers and the like a Wordpress CMS made the most sense as it's so straightforward, but that has some implications on flexibility. It became necessary to strip out most of the stock code and completely change the layout (beyond recognition, really), and that's before I've even finished the responsive part.Perhaps the coolest little trick though has been a jQuery widget that folds out and allows the body text to be dynamically resized from a 'normal' size to about a third larger. The problem with things like that is that they also require CSS3, so part of the next phase of work is going to have to include Modernizr or a similar tool to cater for IE7 and 8 users as the Society has far more than the average number of users running those browsers. Given that Microsoft has all but given up on IE6, pending client approval I think a prompt to upgrade will be served as per <a href="http://www.ie6countdown.com/">this now infamous advice from Microsoft</a>.