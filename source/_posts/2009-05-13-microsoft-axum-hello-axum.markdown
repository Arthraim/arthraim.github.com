---
comments: true
date: 2009-05-13 18:33:23
layout: post
slug: microsoft-axum-hello-axum
title: Microsoft Axum 初探 —— Hello Axum
wordpress_id: 31
categories:
- Programing
tags:
- .net
- Axum
- Hello World
- Microsoft
---

虽然是针对Windows7的，不过还是迫不及待的想写一个Hello,world出来，于是来搞搞吧。




**1、下载Axum.msi，并安装。**




![](/images/uploads/zb/2009-05-13_AxumInstall1.png)







**2、新建Axum语言工程。**




安装好之后，直接打开Visual Studio 2008，可以看到，新的语言Axum出现在列表里，并且有工程类库和控制台程序两个工程可以新建了。




![](/images/uploads/zb/2009-05-13_vs2008axum1.png)







**3、看看Axum的语法规则。**




新建了一个Console Application之后，速度看看它generate了一些什么代码。[如下图]




果然不出所料的一头雾水啊，domain是个什么存在，agent又是什么呢？ :: 是什么？C++的domian的概念吗？那 <-- 这又是什么。虽然看上去有些像C家族的语言，不过一上来真是一头雾水啊。从微软的高亮习惯，似乎可以猜出一些什么东西来。







![](/images/uploads/zb/2009-05-13_GenerateCode.png)







**4、下载Programmer's Guide。**




还是老老实实的看看官方做法吧，下载一份[官方指导](http://download.microsoft.com/download/B/D/5/BD51FFB2-C777-43B0-AC24-BDE3C88E231F/Axum%20Program)来看看。







![](/images/uploads/zb/2009-05-13_Programmer_Guide.png)







**5、Ctrl + C, Ctrl + V 一下，写出我们第一个Hello,Axum～**




看上去调用System命名空间里的方法和C#是一样的吧～ 那么试试看ReadLine什么的。好吧的确一样的。




![](/images/uploads/zb/2009-05-13_HelloAxum.png)




**6、编译出来的bin文件**




![](/images/uploads/zb/2009-05-13_bin.png)







究竟Axum，带给了我们什么新特性呢？继续研究……



