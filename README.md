Static site content for Kloppenborg.net
=====

This website uses the [Jekyll-Bootstrap-3](https://github.com/dbtek/jekyll-bootstrap-3)
system released under the [MIT](http://opensource.org/licenses/MIT) license.

## A reminder of some common functionality

### Start the server locally:

```
jekyll serve
```

### Create a (blog) post:

```
rake post title="Hello world"
```

### Create a page:
```
# Make a generic page
rake page name="about.md"
# Make a nested page
rake page name="pages/about.md"
# Make the pages/about/index.html page
rake page name="pages/about"
```

### Add an item to the navigation menu:

Add the page to the navigation group in the YAML frontmatter.
We have a custom parser located in `_include/navigation` that will parse the
URL to generate submenus"

```
---
layout: page
title: Some page title
group: navigation
---
{% include JB/setup %}
```


