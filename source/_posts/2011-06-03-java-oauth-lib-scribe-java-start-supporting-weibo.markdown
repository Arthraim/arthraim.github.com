---
comments: true
date: 2011-06-03 23:41:22
layout: post
slug: java-oauth-lib-scribe-java-start-supporting-weibo
title: java语言oauth库scribe-java支持新浪微博
wordpress_id: 747
categories:
- Programing
tags:
- github
- Java
- oauth
---

好吧这是一篇蛋疼的文章, 因为本人第一次成功pull request所以比较鸡冻. [scribe-java](https://github.com/fernandezpablo85/scribe-java)是一个java语言的oauth库, 代码很干净利落很容易扩展而且用起来很方便. 因为[之前文章](/something-about-oauth/)提到过的工作, 所以对scribe-java做了[一些改造](https://github.com/Arthraim/scribe-java).




事实上很多改造是因为scribe-java的确还存在一些问题, 比如oauth参数不支持querystring等, 所以改造工作不可避免. 不过随着作者非常快速的更新, 现在它已经大大改善了这些方面的功能. 回过头来再看自己的folk, 事实上也完全没有必要了.




当然很希望国内的开发者在pom.xml里写上scribe-java就可以直接支持国内的各个微博, 所以我整理了一下我的分支正式的发起了pull request. 经过几次交流, 最终 新浪微博 / 网易微博 / 搜狐微博 的相关支持已经正式纳入了原作者的版本中得到支持了. 另外我一起提出请求的QQ微博和人人2.0最终因为代码的修改太大, 作者没有采纳(如果你要使用, 可以下载[我的folk](https://github.com/Arthraim/scribe-java)).




具体用法欢迎参考[这个例子](https://github.com/fernandezpablo85/scribe-java/blob/master/src/test/java/org/scribe/examples/SinaWeiboExample.java) :) 开源是个尝到甜头就像继续下去的工作, 哪怕你的贡献再渺小, 也非常的美好. 让我鸡冻的感谢一下原作者Pablo Fernandez, 来自阿根廷.



