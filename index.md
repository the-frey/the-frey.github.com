---
layout: page
title: Alex Lynham
tagline: One small step for a developer...
---
{% include JB/setup %}


    
    
    


## Posts.

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>




