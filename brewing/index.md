---
layout: page
title: brewing
tagline: posts about brewing
---
{% include JB/setup %}


<h3 class="category">Brewing Posts</h3>

<ul class="posts">
	{% if site.categories.brewing.count == 0 %}
		<span>No posts yet</span>
	{% endif %}
	{% for post in site.categories.brewing %}
    	<li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
	{% endfor %}
</ul>


