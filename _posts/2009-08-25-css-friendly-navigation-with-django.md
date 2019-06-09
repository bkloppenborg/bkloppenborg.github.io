---
layout: post
title: "Django: CSS-Friendly Dynamic Navigation Menus"
date: 2009-08-25T12:00:49-00:00
draft: false
author: "Brian Kloppenborg"
categories: ['web-development']
tags: ['django', 'navigation']
---
{% include JB/setup %}

<!--
TODO: Broken links
-->

One design principle I have read is to keep your navigation menu to a depth of
two or less. Following this principle, the code I will discuss below will
produce a navigation menu that will have a maximum of one pop-out for each
primary link. The key to getting the navigation menu to work is to have the
output be in the form of an HTML unordered list so that CSS can be applied to
the `<ul>` and `<li>` tags. Django has a filter, entitled
[`unordered_list`](http://docs.djangoproject.com/en/dev/ref/templates/builtins/#unordered-list),
that can parse an array of strings (or nested arrays of strings) into an
unordered list. I copied the `defaultfilters.py` from the Django source code and
removed everything except the `unordered_list` function (and any necessary import
lines) and then rewrote the `unordered_list` function to work on a list of
`MenuItems` objects. The modifications to the `unordered_list` function were few
and basically involved changing the output method from a string to a call to the
yet-to-be-written `MenuItem.render()` function. A copy of that file can be found
in my
[navigation_filters.py](http://finiteline.homeip.net/blog/wp-content/uploads/2009/08/navigation_filters.py)
file. Place this fine in the "navigation/templatetags" directory along with
blank file called `__init__.py` so the directory will be treated as a Python
class. Just like the `unordered_list` function, `navigation_list` takes an array
(or a nested array), but unlike `unordered_list`, `navigation_list` will only
output from MenuItems objects. Now we write a template that uses the new
`navigation_list` filter:

{% raw %}
    <div id="navmenu">
    	<ul>
    		<li>Navigation</li>
    	{% load navigation_filters %}
    	{% if nav_menu %}
    		{{ nav_menu|navigation_list }}
    	{% else %}
    		<li>No Navigation Menu</li>
    	{% endif %}
    		<li>nbsp;</li>
    	</ul>
    </div>
{% endraw %}

With the template and filter written, now all that needs to be done is to create
the navigation module. The ideal method of storing the navigation menu would be
in a hierarchical list (see 
[djangosnippets](http://www.djangosnippets.org/snippets/746/), and the
[django-mptt](http://code.google.com/p/django-mptt/) project), but the status of
the `django-mptt` project seemed unnecessarily complicated for this task.
Additionally, a full hierarchical list would encourage the creation of
navigation lists more than two deep, something against design principles.

I therefore created a very simple semi-hierarchical model to store the
navigation menu that also contains the `MenuItem.render()` function I mentioned
above:

    from django.db import models
    
    # Create your models here.
    class MenuItem(models.Model):
        ParentID = models.ForeignKey('self')
        text = models.CharField(max_length=20)
        url = models.CharField(max_length=200)
    
        """ Returns the title of the news item. """
        def __unicode__(self):
            return self.text
    
        def render(self):
            return '<a href = "' + self.url + '">' + self.text + '</a>'</pre>
    

I enabled the administrative interface for this class and inserted some junk
data. I then wrote a context processor to pull the necessary information from
the database:

    from django.template import Context
    from finiteline.navigation.models import MenuItem
    
    def navigation_context(request):
        # Define a variable in which we store the output list
        output_list = []
    
        # Run a query to get the top-level objects
        nav_menu = MenuItem.objects.filter(ParentID=1).order_by('text').exclude(pk=1)
    
        # For each top level object, run a query to get the subitems.
        for item in nav_menu:
            sub_menu = MenuItem.objects.filter(ParentID=item.id).order_by('text')
    
            output_list.append(item)
    
            if sub_menu.count > 0:
                sub_list = []
    
                for subitem in sub_menu:
                    sub_list.append(subitem)
    
                output_list.append(sub_list)
    
        c = Context({'nav_menu': output_list})
        return c</pre>

After including the navigation template mentioned above, I included
"navigation/base.html" in another page and opened it up in a web browser. The
output looks like the image shown to the right. After applying CSS to the `<ul>`
and `<li>` tags, the navigation menu changes considerably and even supports
pop-out navigation!
