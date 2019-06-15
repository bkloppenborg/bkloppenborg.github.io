---
layout: post
title: "Django and dynamic site-wide navigation menus"
date: 2009-08-24T12:00:03-00:00
draft: false
author: "Brian Kloppenborg"
tags: ['django', 'navigation']
categories: ['web-development', "old-content"]
---
{% include JB/setup %}


When I started doing web development professionally back in 2004 my first
application of server-side includes was for navigation menus.
They are one of the most critical components of any website and thus it
was the first "real" task I attempted to tackle with Django.
The problem is that using the template system you can easily encode a static
navigation menu in a base template and then inherit that for all subsequent
pages, but what if you want a dynamically-generated navigation menu perhaps
adding in user-specific links after login? The answer to this question turned
out to be more complicated to track down than to actually implement. 

In my Django project, I first created a new app entitled 'navigation' and added
it to INSTALLED\_APPS in the settings.py file. We won't do anything with this
app for a while, but we'll need it to be there. Lets start by looking at my
template for my HTML pages called "base.html":

Just like with server-side-includes, you can include your navigation menu by an
include directive:

{% raw %}
    {% include 'navigation/base.html' %}
{% endraw %}

Whereas the navigation template (navigation/base.html) looks like this:

{% raw %}
    <ul>
    {% if nav_menu %} {% for entry in nav_menu %}
      <li><a href="#">LinkText</a></li>
    {% endfor %} {% else %}
      <li>No Navigation Menu</li>
    {% endif %} 
    </ul>
{% endraw %}

I've actually copied the Django templates into a directory called 'templates'
and then updated `TEMPLATE_DIRS` in the `settings.py` file accordingly. The problem
is that with this implementation, you must include a context for the navigation
menu (`nav_menu` above) in every page that will have the navigation menu
displayed... too much work. A simple way to fix this is to override the default
context processors to cause some contexts to be automatically generated when a
page is requested. To do so, add the add the following to your settings.py file:

    TEMPLATE_CONTEXT_PROCESSORS = (
        "django.core.context_processors.auth",
        "django.core.context_processors.debug",
        "django.core.context_processors.i18n",
        "django.core.context_processors.media",
        "navigation.context_processors.menu_context"
    )

The `django.core.context_processors` are the default options as specified 
[in Django's API documentation](http://docs.djangoproject.com/en/dev/ref/templates/api/) 
and my own custom context processor is specified by
`navigation.context_processors.menu_context`. Now we need to specify the
context to be generated. In the `navigation/context_processors.py` we need to
create a context to our navigation object:

    def menu_context(request): 
        nav_menu = "NAV" c = Context({'nav_menu': nav_menu})

        return c

I have not actually written any other code in the navigation module so at the
moment I'm just returning a string that, when processed by the navigation
template, should result in three navigation links showing up. Now that we have
set up automatic generation of contexts, how do we use them? Well, instead of
using the Context class to generate contexts in our views, we use the
RequestContext class:

    def some_view(request):
    
        # ...
        c = RequestContext(request, ...)
        return HttpResponse(t.render(c))</pre>

Or if you use the `render_to_response` function, you need to supply an optional
parameter, the `context_instance`:

    def menu_context(request):
    
        nav_menu = "NAV"
 c = Context({'nav_menu', nav_menu})
        return c

See the 
[Context Documentation](http://docs.djangoproject.com/en/dev/ref/templates/api/#subclassing-context-requestcontext)
for more information. In my next post I'll describe how to design a very simple
model to store your navigation menu and how to export the data into a
CSS-compatible navigation menu.
