---
comments: true
date: 2010-06-05 15:07:49
layout: post
slug: pydev-new-version-supports-django
title: Pydev新版本支持Django
wordpress_id: 451
categories:
- Programing
tags:
- django
- eclipse
- Pydev
- Python
---

偶然发现pydev已经更新到了新版本，新版本里原生支持了django。这个版本应该已经更新了快有1个月了吧。pydev是eclipse开发python的利器，是对eclipse最好的扩展；而Django是python web框架中最受欢迎的一个。pydev如今原生支持Django，使得Django的使用更加方便了。




[![](/wp-content/uploads/pydev_banner.gif)](/wp-content/uploads/pydev_banner.gif)[![](/wp-content/uploads/django-logo.jpg)](/wp-content/uploads/django-logo.jpg)




以前的做法是创建pydev项目，在项目目录下django-admin.py startproject，然后把新建的目录设为src，再自己设置Run和Debug，使用manage.py runserver 0.0.0.0:8080。




如今pydev会自动在新建Django项目的时候startproject，自动设为src，和我们手动做的其实都是一样的。不过还会自动添加DJANGO_MANAGE_LOCATION和DJANGO_SETTINGS_MODULE两个字符串替换变量（string substitution variable）分别指向项目目录下的manage.py和settings.py。




当然，以前手动处理的项目，也可以使用右键-pydev-Set As Django Project把原有的项目转换为Pydev Django项目。当然一样要确认下是不是有DJANGO_MANAGE_LOCATION和DJANGO_SETTINGS_MODULE两个变量。




[![](/wp-content/uploads/2010-06-05_pydev_django_vaviables.png)](/wp-content/uploads/2010-06-05_pydev_django_vaviables.png)




详细情况参见[官方说明](http://pydev.org/manual_adv_django.html)。




以上。
