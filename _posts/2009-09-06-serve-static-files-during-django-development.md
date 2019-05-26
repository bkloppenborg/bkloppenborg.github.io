---
layout: post
title: "Django: Serving Media Files During Development Process"
date: 2009-09-06T14:35:19-00:00
draft: false
author: "Brian Kloppenborg"
tags: ['django']
categories: ['web development']
---
{% include JB/setup %}

While developing in Django, it is often useful to serve media/static files to
ensure design templates are displayed correctly. In order to do so, you need to
add a custom line to the `urls.py` file:

    # ...
    
    if settings.DEBUG:
        urlpatterns += patterns('',
            (r'^site_media/(?P&lt;path&gt;.*)$', 
             'django.views.static.serve', 
             {'document_root': '/path/to/media'}),
        )

In addition to this modification, I also like to enable directory listings
by modifying the `settings.py` file as follows:


    from django.conf.urls.defaults import *
    from django.conf import settings    
    
    # Uncomment the next two lines to enable the admin:
    from django.contrib import admin
    admin.autodiscover()
    
    urlpatterns = patterns('',
        ...
        # Uncomment the next line to enable the admin:
        (r'^admin/', include(admin.site.urls)),
    )
    
    if settings.DEBUG:
        urlpatterns += patterns('',
            (r'^%s(?P&lt;path&gt;.*)$' % settings.MEDIA_URL[1:],
             'django.views.static.serve',
             {'document_root': settings.MEDIA_ROOT, 'show_indexes': True}),
        )

Now, I wanted all of my media files served from the `/media` directory on my
webserver, unfortunately the admin interface handles all incoming requests to
`/media` by default. Therefore, I moved the admin directory in my `settings.py`
file and cleaned things up a little so I can quickly deploy my website:

    if(DEBUG):
       # Settings on Development Server
        MEDIA_ROOT = '/path/to/development/media/'
    else:
        MEDIA_ROOT = '/path/to/deployed/media/'
    
    MEDIA_URL = '/media/'
    ADMIN_MEDIA_PREFIX = '/media/admin/'</pre>

Further information on serving static content with the development server can be
found on the
[Django Static File Documentation](http://docs.djangoproject.com/en/dev/howto/static-files/) page.
