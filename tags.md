---
layout: page
title: Tags
permalink: /tags/
---

<h2>{{ page.tag }}</h2>

<ul>
{% for post in site.posts %}
  <li><a href="{{ post.url }}">{{ post.title }}</a> ({{ post.date | date_to_string }} | Tags: {{ post | tags }})</li>
{% endfor %}
</ul>

{% include tagcloud.html %}
