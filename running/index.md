---
layout: page
title: running
tagline: posts about running
---
{% include JB/setup %}


<h3 class="category">Recent Posts</h3>

<ul class="posts">
  {% for post in site.categories.running %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

<h3 class="category">Running Log</h3>

<ul class="posts">
  {% for post in site.categories.runlog limit:10%}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
  <a href="{{ BASE_PATH }}/running/traininglog">view more</a><hr/>
</ul>
