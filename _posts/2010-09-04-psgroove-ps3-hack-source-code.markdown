---
comments: true
date: 2010-09-04 14:33:18
layout: post
slug: psgroove-ps3-hack-source-code
title: 破解PS3源代码psgroove
wordpress_id: 582
categories:
- Video Game
tags:
- 破解
- github
- Playstation
- PS3
- psgroove
- PSjailbreak
---

PS3四年了，一直没破解，之前George Hotz，那个完成iPhone越狱的少年号称破解了PS3，利用在主板上焊上一个按钮，在特定开机时刻发送特殊的信号实现了破解，但是源代码也公布了。但是后来geohoz在博客宣称只进展到这里不再继续了，希望有心人加入这个接下来的工作中。之后发生的事情有些匪夷所思，各界开始谩骂geohoz，称其没有真正破解PS3，甚至诋毁他的能力。事实上当时的代码的确已经破解了PS3的，只是因为还要一个硬件的支持，距离量产更是很远！但的确是有人在论坛上成功的。




再后来比较爆炸的新闻就是PSjailbreak，利用一个类似U盘一样的小小的USB设备就可以完成破解PS3的工作。利用这个PSjailbreak，可以读硬盘上的镜像文件来玩游戏。随着jailbreak消息越来越多，国内某处也号称开始生产，jailbreak的价格也是非常的可怕，看到过一个价格是140的美刀，这貌似都要超过。




今天看到一则[消息](http://www.h-online.com/open/news/item/PS3-hack-source-code-published-1071444.html)比较开心（索尼也很开心），**一个叫做psgroove的代码在[github](http://github.com/psgroove/psgroove)上开源了**，这是个什么玩意儿呢。其实就是一些代码，可以在AT90USB系列的开发板上运行。把设备接到PS3上后，第一时间向PS3发送一些代码，这些代码本来是加载USB外部设备包括电源连接数等等信息，如果这些代码非常多非常长，就会溢出，听到溢出后面就不用说了，我们自己的代码就可以被跳出来跑了。




现在这个代码对3.41内核有效，我有点想对自己家PS3动手的冲动了。




以上……




**Update**: 有人把这个代码移植到了N900和palm pre上。另外，如我所说，索尼真的很开心，昨天发布了3.42就是修补这个漏洞的。




Update 2: 昨天看到有人把PSgroove移植到了[可编程计算器](http://brandonw.net/ps3jb/)上，然后还有[andoird](http://www.redmondpie.com/how-to-jailbreak-playstation-3-ps3-with-psfreedom-using-android-nexus-one-htc-desire/)，今天又看到有人移植到了[iPhone和iPod上](http://www.redmondpie.com/jailbreak-playstation-3-ps3-with-iphone-3g-ipod-touch-using-psfreedom-psgroove-video/)，开源就是好啊，越来越靠谱了~
