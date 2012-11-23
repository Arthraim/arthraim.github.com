---
comments: true
date: 2010-06-06 05:26:11
layout: post
slug: django-static-files-serve
title: Django的静态文件服务
wordpress_id: 458
categories:
- Programing
tags:
- django
- Python
---

说django的静态文件说起来很简单，不过我还真是碰到了形形色色的问题并且追求完美。说起来一般做法用过django的应该都熟悉部署环境中只要利用web server去处理静态文件就可以了；调试时可能稍微特殊一点，按照[官方文档](http://docs.djangoproject.com/en/dev/howto/static-files/)所说，要在urls里加上这样的代码。



    
    if settings.DEBUG:
    	    urlpatterns += patterns('',
    	        (r'^media/(?P<path>.*)$', 'django.views.static.serve',
    	            {'document_root': settings.MEDIA_ROOT}),
    	    )




另外，settings里要做三个关于media的配置





	
  * 配置MEDIA_ROOT指向media文件夹的本地绝对路径

	
  * 配置MEDIA_URL为/media/

	
  * 配置ADMIN_MEDIA_PREFIX为/amedia/




这是最正确的做法，按照这样做的话部署的时候什么代码都不用改动就可以保证media服务正常了。（最重要的是直接使用/media/写模板文件就好了）




不过因为ADMIN_MEDIA_PREFIX的设置如果按照默认的/media/，那么就会使得调试时无法正常服务静态文件。如果MEDIA_ROOT不配置，ADMIN_MEDIA_PREFIX改成./media/，那么站点的静态文件能正常服务，但是admin页面的静态文件就错误了。如果不配置MEDIA_ROOT，也不修改ADMIN_MEDIA_PREFIX（就像默认的一样），那么一样只要在urls.py里写上之前的代码，并把setting.MEDIA_ROOT直接替换成本地的绝对路径，那么就能正常服务静态文件，唯一要做的，是不使用r'^media/(?P<path>.*)$'，而改一个诸如r'^dmedia/(?P<path>.*)$'之类的其他路径，我我很不喜欢这个方式，能解决问题，但是部署的时候我就是看得不顺眼，要改的话就要改所有模板、py中的所有路径。




总之上面正确方法之后说的所有东西都是我碰到过的情况，按照文章一上来的做法做，那么调试到部署，一切都很简单。如有错误，或有更标准的做法，希望您留言。




以上。
