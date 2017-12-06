---
layout: page
title: Astrophysicist & Software Developer
tagline:
---
{% include JB/setup %}


<!--- Enable markdown parsing inside of a div using markdown="1" --->
<img class="img-responsive img-thumbnail"
    style="float: right; width: 55%; margin: 5px;"
    src="/images/brian-at-chara.jpg"
    alt="A photograph of Brian Kloppenborg at CHARA" />
**Hi I am Brian Kloppenborg. I am an astrophysicist with a knack for everything
technological.**

I am Research Faculty at Georgia Tech Research Institute and founder of
[Pratum Labs, LLC](http://pratumlabs.com) a data science and machine learning
company.

In my spare time I conduct astrophysical research as part of my adjunct
professor position in [Georgia State University's](http://gsu.edu)
[Department of Physics and Astronomy](http://phy-astr.gsu.edu/).
Most of my research focuses on resolved observations of stellar surfaces,
eclipsing binaries, and novae using optical interferometry.

While not working I like to explore the outdoors, experiment with new technology,
come up with innovative solutions to otherwise benign problems, and play old
video games.

-----

## Recent posts

<ul class="posts">
  {% for post in site.posts limit:5%}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
