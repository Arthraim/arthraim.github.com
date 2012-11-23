---
comments: true
date: 2009-05-20 23:38:37
layout: post
slug: s60v5-ide-and-helloworld
title: S60v5 IDE的搭建 和 Hello,World的实现
wordpress_id: 39
categories:
- Programing
tags:
- Hello World
- IDE
- S60
---

入手了5800之后，在闲暇之余就很想尝试着能不能自己开发一些东西，在V2和V3的时代都有过这样的念头，不过当时nokia的支持还是比较薄弱的。在收购的Symbian之后，nokia的支持，让现在的开发变得方便而且快捷，并且提供的选择也是非常的丰富。我简单的了解了一些S60应用开发的途径，大概列举一下有以下几种（不乏很多其他方法，诸如widget等）






  * JAVA ME


  * Symbian C++（或者Open C/C++）


  * QT for S60


  * Flash Lite




我将搭建的是C++的环境，版本取决于你的sdk版本。首先要做一些准备。(S60的开发是不一定在Windows下进行的，不过我这里默认是Windows了说)






  * 一台电脑


  * 一台S60的终端


  * [JAVA运行环境](http://www.java.com/zh_CN/download/manual.jsp)


  * [ActivePerl](http://www.activestate.com/activeperl/downloads/)


  * [S60 SDK](http://www.forum.nokia.com/info/sw.nokia.com/id/ec866fab-4b76-49f6-b5a5-af0631419e9c/S60_All_in_One_SDKs.html) (这里有一个所谓all in one版本，包括了JAVA等其他的开发手段)


  * [Carbide.C++](http://www.forum.nokia.com/info/sw.nokia.com/id/dbb8841d-832c-43a6-be13-f78119a2b4cb.html)




![](/upload/2009-05-20_Package.png)




做完准备之后，就按顺序装下来，Java应该一般人都有吧，没有的话装个也简单，最新版的jre，不需要完整的jdk，只是IDE是JAVA环境运行的而已。除非你是采用JAVA开发。




另外ActivePerl也是必备的，不然会出现提示。




![](/upload/2009-05-18_perl_needed.png)




5th Edition SDK大概是600+MB，安装过程可谓相当的缓慢且痛苦难熬，我的电脑大概装了1个多小时才把sdk装好而已。安装目录会要求英文并且没有空格之类的，注意一下就好了。装完之后也是一个比较大的玩意儿了。




![](/upload/2009-05-18_Big!.png)




至于Carbide.C++，是基于eclipse的产物，所以其实用eclipse也是可以开发的，安装sdk时可以选择安装eclipse插件。




![](/upload/2009-05-18_JAVAsupport.png)




改装的都装了，打开Carbide看看究竟是什么个情况。不出意外就可以新建Symbian C++ OS Project了，选择带有设计器的有GUI的工程试试看，能不能做出点东西来。跟着向导走，会要求选择sdk，从你选择的里面选吧～ 像我就只有一个5th Edition了。




图形上可以选择的分辨率很多，那么5800XM就是320×640。界面设计还是非常的让人兴奋的，旁边的控件也是一堆一堆，看看网上的我文档，各种各样的控件真的是非常让人兴奋的，究竟怎么用慢慢研究，先看看设计界面。







![](/upload/2009-05-20_GUIdesign.png)




随便放几个控件，然后编译----运行，在模拟器中运行！成功！







![](/upload/2009-05-20_HelloWorld.png)




支持触摸控制，打个包就可以在手机里运行了吧～ 哈哈，希望看到的大家也一切顺利～ 有问题可以留言。因为虽然看上去很方便，不过其实弄得时候也遇到了一些问题的～ 加油！




补充一张朦胧的照片，在我的实机上跑的效果。IDE还提供了在线联调等很强大的功能，值得一玩～




![](/upload/2009-05-20_HelloWorld_onPhone.jpg)







再补充一次，今天申请的开发者17权限证书终于拿到了，自签名了几个原来没有装上的软件，终于可以截图和上twitter了~ 那么看看HelloWorld的真实截图情况吧~ 不错~




![](/upload/Scr000001.jpg)
