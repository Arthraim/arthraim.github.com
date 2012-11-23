---
comments: true
date: 2011-05-29 20:46:07
layout: post
slug: bottle-i-love-this-framework
title: bottle-非常喜欢这个框架
wordpress_id: 742
categories:
- Programing
tags:
- bottle
- framework
- mongodb
- Python
- web
---

大概是我火星了，直到2天前我才知道有bottle这样一个框架。鉴于对自己的翻译能力没有什么信心，就摘抄[bottle官网](http://bottlepy.org)上对自己介绍的原文过来吧。




> 
	
> 
> Bottle is a fast, simple and lightweight WSGI micro web-framework for Python. It is distributed as a single file module and has no dependencies other than the Python Standard Library.
> 
> 





坦率说因为工作中是使用spring的缘故，其实真正用python的web框架机会并不多。相对接触的比较多的是django，无论是工作中还是生活中写点小东西的时候我还是比较愿意使用django，django+mongoengine，可以快速的使用mongodb来做存储，具体就不细说了这不是重点，[之前的文章](http://artori.us/use-mongodb-with-django/)中提到过。至于pylons, web.py或者其他的，大概看了一眼文档就没啥特别的感觉就没有深究下去。




[![](http://artori.us/wp-content/uploads/logo_nav.png)](http://artori.us/bottle-i-love-this-framework/logo_nav/)




今天主角是bottle，早上醒来迷迷糊糊在reader上看了[一个充满印度腔的视频](http://simple-is-better.com/news/detail-292)，讲用bottle框架5分钟创建一个wiki。就是这个视频让我对bottle一见钟情。可以看一下官网的helloworld




[![](http://artori.us/wp-content/uploads/2011-05-29_bottle_hello_world.png)](http://artori.us/bottle-i-love-this-framework/2011-05-29_bottle_hello_world/)




好像做一个restful的东西好轻松的样子，直到我花了2个小时认真的读了他的[这一页](http://bottlepy.org/docs/dev/tutorial.html)文档，我才觉得这真的太棒了！搭建一个web应用真的是手到擒来鸟。dict直接输出为json，multipart上传文件直接从request.files里面读取，cookie以及加密的cookie等等偷懒的feature真是不少。




内建的template相对要蛋疼一点，我都没有找到loop，不过支持mako很简单，和内建的用法几乎是一模一样的。




各种给力不多说了，现在自己在写一个用bottle+mongo的应用玩，另外学习的时候写了[一个例子](https://gist.github.com/994641)，也传到了github上，愿意的话可以看看，里面有很多让人高兴的地方。如果你和我一样有做一个web应用的冲动，不妨试试看bottle！




以上。



