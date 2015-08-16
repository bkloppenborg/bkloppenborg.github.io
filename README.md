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

Modify the `_data/navigation.yml` file, the syntax is pretty obvious.
