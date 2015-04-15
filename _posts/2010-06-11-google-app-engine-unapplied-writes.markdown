---
comments: true
date: 2010-06-11 02:46:03
layout: post
slug: google-app-engine-unapplied-writes
title: Google App Engine的Unapplied Writes
wordpress_id: 460
categories:
- internet
tags:
- App Engine
- Google
- spam
---

话说刚才收到一封来自Google App Engine的邮件，大致内容是说在2010-5-25日的时候服务有我的某个程序的服务器发生了大约长达50分钟的停机，于是因此在这段时间里我的应用依然产生了数据库的写入操作，这时候产生的数据被记录到了别的服务器上，并且被称为Unapplied Writes。主要在告诉我如何挽救这些数据的同时，还为给我带来的不便说了抱歉。下面是Email的全文~




[![](/images/uploads/wp/2010-06-11_GAE_uapplied_writes.jpg)](/images/uploads/wp/2010-06-11_GAE_uapplied_writes.jpg)




于是让我百思不得其解的事情就是为什么jmini-arthraim这个应用有这么NB，这么巧合，在断电的时候有数据写入还造成了数据不同步呢？让Arthur来帮大家回忆一下jmini-arthraim究竟是个什么东西。




在去年我写了一篇《[用Java写个Google App Engine应用](http://artori.us/google-app-engine-apps-in-java/)》的文章介绍GAE的JAVA支持。程序本身很简单，完全没有任何参考价值，只是我对文章介绍了很多程序之外的软件安装啊环境搭建这样的简单的步骤。jmini-arthraim相当于一个简单的留言板，简单来说就是用户提交title content信息，写入数据库这样子。大概不知道这个程序和哪个刷spam的程序犯冲了，或是有人无聊到针对我对程序做了刷spam的程序，总之几个月来，这个应用一直忍受着常人难以忍受的委屈。




我去后台看了一下这个应用的数据，去掉前11条是真正有朋友来测试的消息以外，从去年11月19日开始就有无数的spam数据开始刷了。真是场面何其壮观啊！总共有多少条记录呢？133,876（狂汗）估计这是我目前为止自己创建过的信息量最庞大的数据库了。如下图所示：




[![](/images/uploads/wp/2010-06-11_GAE_store_statistics.jpg)](/images/uploads/wp/2010-06-11_GAE_store_statistics.jpg)




我写这篇文章的原因有3：






  1. Google很负责，这个事情告诉我们GAE上跑的程序数据是比较安全的。


  2. Spamer很强大，不论是撞到了哪个spamer的枪口上，总之这个兼容性可以强大到向我这样一个应用开火，那也的确强大了。


  3. 娱乐一下~ 今天晚上22点世界杯就开幕了，遇上这样的事还真是挺乐的。hoho




以上。
