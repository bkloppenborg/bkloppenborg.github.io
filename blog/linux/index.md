---
layout: page
title: Blog posts about Linux
header: Posts about Linux
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.categories["linux"] %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo;
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
