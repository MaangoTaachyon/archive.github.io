---
title: "Posts by Year"
permalink: /posts/
layout: posts
author_profile: true
---

<ul>
{% for post in site.posts %}
  {% assign currentdate = post.date | date: "%Y" %}
  {% if currentdate != date %}
    <h3>{{ currentdate }}</h3>
    {% assign date = currentdate %} 
  {% endif %}
    <a href="{{ post.url }}">{{ post.title }}</a>
    {{ post.excerpt }}
{% endfor %}
</ul>
