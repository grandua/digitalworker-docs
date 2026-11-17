---
layout: default
title: Blog — DigitalWorker
permalink: /blog/
redirect_from:
  - /blogs/
  - /blogs.html
  - /blog.html
---

# DigitalWorker Engineering Blog

[Engineering Deep Dive]({{ '/Engineering-Deep-Dive/' | relative_url }}) · [Public Demo](https://github.com/grandua/Digital-Worker-Demo)

{% if site.posts.size == 0 %}
No posts published yet.
{% else %}
  {% for post in site.posts %}
<h2><a href="{{ post.url | relative_url }}">{{ post.title }}</a></h2>
<p><small>{{ post.date | date: "%B %-d, %Y" }}</small></p>
{{ post.excerpt }}
<p><a href="{{ post.url | relative_url }}">Read article &rarr;</a></p>
<hr>
  {% endfor %}
{% endif %}
