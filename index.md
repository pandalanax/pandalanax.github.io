---
layout: default
title:  "pandalanax"
---

This is a cheatsheet / blog / wiki for my projects.

<H3>Posts</H3>

{% for post in site.posts %}{% if post.draft != true %}
- [{{post.title}}]({{ post.url | prepend: site.baseurl }}) ({{ post.date | date: "%b %-d, %Y" }}) {% endif %}{% endfor %} 

