---
layout: post
slug: optimized-png-in-xcode
title: Xcode优化过的PNG
date: 2012-12-12 00:07
comments: true
categories:
- Programing
tags:
- xcode
- png
---

开始做iOS应用就有一个“公理”，**图片素材要使用png格式**，至于公理是怎么形成的完全不知道，只是听说在官方文档里提到过一句：苹果会对png进行优化。

为什么优化？谁优化的？什么时候优化的？怎么优化的？

和所有的魔术一样，说穿了就不好玩了。一切的根源是iPhone的显存。iPhone的vRAM在存放单个像素的颜色的时候，并不是按照传统的“红-绿-蓝”这样的顺序排列的，而是“蓝-绿-红”，即我们常说的RGB，在iPhone的显存里是BGR。并且，没有alpha通道。

另一边，png格式按照“红-绿-蓝”的顺序描述颜色，并且支持alpha通道的半透明，RGBA四个通道各占1个字节。

* 为什么优化？
  因为一边RGB一边BGR，一边有alpha一边没有alpha。
* 谁优化的？
  文章标题已经剧透了，Xcode优化的。
* 什么时候优化的？
  Xcode在编译时，会对png资源进行优化。
* 怎么优化的？
  优化做了两件事：
   1. 把png里所有的RGB颜色转成BGR顺序
   2. 把png里所有的alpha通道先和RGB三通道先乘好（比如R:1 G:1 B:1 A:0.5的颜色直接转成 R:0.5 G:0.5 B:0.5）

这样最终设备在运行时渲染这些颜色的时候，不需要任何处理，一个汇编语句就把数据丢尽显存里了。

PS: [这里](http://developer.apple.com/library/ios/#qa/qa1681/_index.html)还有一个手动转换，和还原的办法

via: [http://iphonedevelopment.blogspot.jp/2008/10/iphone-optimized-pngs.html](http://iphonedevelopment.blogspot.jp/2008/10/iphone-optimized-pngs.html)