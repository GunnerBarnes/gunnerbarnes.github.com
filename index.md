---
layout: page
title: 
tagline: 
---
{% include JB/setup %}


<h3 class="category">Recent Posts:</h3>

<ul class="posts">
  {% for post in site.posts %}
  	{% if post.category != "runlog" %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>



