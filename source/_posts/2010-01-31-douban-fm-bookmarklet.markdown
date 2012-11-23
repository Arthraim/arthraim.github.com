---
comments: true
date: 2010-01-31 13:08:12
layout: post
slug: douban-fm-bookmarklet
title: 豆瓣电台的bookmarklet
wordpress_id: 141
categories:
- internet
tags:
- bookmarklet
- douban.fm
- 豆瓣
---







最近真是越来越喜欢豆瓣了，douban.fm这个豆瓣电台更是变成了系统里唯一的音乐播放器~ 不过每次听豆瓣好像都不是很方便，因为总要现在地址栏输入douban.fm，然后点一个链接…… 于是今天Inspect Element了一下，发现也就是href里面的一段js代码，那就好办了，直接可以做个书签啊。




![](/upload/2010-01-31_douban_fm_bookmarklet.png)




不就这么简单咩~ 其他浏览器也是一样的，在地址的地方输入这一段JS代码就好了，代码在下面……



    
    
    javascript:if(!window.open('http://douban.fm/radio','radiowin','height=186,width=420,toolbar=no,menubar=no,scrollbars=no,location=no,status=no')){location.href='http://douban.fm/radio'}




So，一个小小的窍门而已，不过蛮方便的不是么~ 现在书签栏里点下豆瓣电台就出来了~ bookemarklet的体验，其实貌似原理都是一样的 - -




以上……
