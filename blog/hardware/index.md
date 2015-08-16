---
layout: page
title: Blog posts about hardware
header: Posts about hardware
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.categories["hardware"] %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo;
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
