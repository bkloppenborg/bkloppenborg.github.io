---
layout: page
title: Blog posts about various software
header: Posts about software
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.categories["software"] %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo;
        <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
