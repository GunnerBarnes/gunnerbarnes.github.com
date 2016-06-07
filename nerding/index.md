---
layout: page
title: nerding
tagline: posts about software engineering
---
{% include JB/setup %}


<h3 class="category">Nerding Posts</h3>

<ul class="posts">
  {% for post in site.categories.nerding %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>



