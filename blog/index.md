---
layout: page
title: Blog posts
---
{% include JB/setup %}

<p>List by <a href="categories.html">Categories</a> | <a href="categories.html">Tags</a></p>

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>

