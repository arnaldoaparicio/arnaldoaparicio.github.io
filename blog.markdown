---
layout: page
title: blog
permalink: /blog/
---

# Useful information and ramblings

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
