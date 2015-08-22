---
layout: page
title: Blog posts by category
header: Posts By Category
---
{% include JB/setup %}

## Categories

<ul class="tag-box inline">
  {% assign categories_list = site.categories %}
  {% include JB/categories_list %}
</ul>

<hr/>

## Posts

{% for category in site.categories %}
  <h3 id="{{ category[0] }}-ref">{{ category[0] | join: "/" }}</h3>
  <ul>
    {% assign pages_list = category[1] %}
    {% include JB/pages_list %}
  </ul>
{% endfor %}

