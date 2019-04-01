---
layout: post
slug: 3d-model-viewer-on-m5stick
title: M5Stick 上的体感 3D 模型查看器
date: 2019-03-31 10:13:49
comments: true
categories:
- Programing
tags:
- M5Stick
- Arduino
- M5Stack
- ESP32
- MPU9250
- C
- C++
---

前段时间做了一个小玩具，如下图所示，也可以看[这个视频](https://m.okjike.com/originalPosts/5c923624f7b0dc00119bff68?username=F65E8535-A827-4B75-8583-EFD4EB5C2671)。写出来记录一下心路历程。

![](/images/uploads/jekyll/m5stick.jpg)

那几日沉迷淘宝推荐流不能自拔，于是在清晨迷糊中买下了一个 [M5Stick](https://docs.m5stack.com/#/en/core/m5stick) 带 MPU9250 的版本。不贵，100 元不到的价格，想着买都买了不玩一玩对不起自己的冲动消费。

## 搭建环境

好久没玩 Arduino 了，上次还是 [13 年做的门铃](https://weibo.com/micbell?is_all=1)。发现 [VSCode 也有插件](https://twitter.com/Arthraim/status/1106172342835675138)，比原来的 Arduino app 可好用多了。

其他的依赖和板子信息按照[官方文档](https://docs.m5stack.com/#/en/quick_start/m5stick/m5stick_quick_start_with_arduino_MacOS)安装即可。

遇到一个坑，刚开始问了淘宝卖家也没解决。Mojave 上连接了 M5Stick 之后，怎么都看不到对应的串口。但系统设备里可以看到这个设备叫

`CP2104 USB to UART Brige Controller`

于是顺藤摸瓜，找到了对应的[驱动](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)，下载安装重启搞定~~ 顺便告诉了淘宝卖家，希望能帮到今后遇到这个问题的人。

## 3D图形

首先有兴趣的是屏幕，一个 128x64 的液晶屏，显示些什么比较有意思的呢。网上的例子多是用来显示纯文本。比如原厂的[几个例子](https://github.com/m5stack/M5Stack/tree/master/examples/Stick)多是用到了 [u8x8](https://github.com/olikraus/u8g2/wiki/u8x8reference)。其实我们的 ESP32 这么强大，为啥没人做做 3D 的图形。

于是经过一番研究，发现图形还得用 [u8g2](https://github.com/olikraus/u8g2/wiki)。另外我并不熟悉 3D projection 的算法，在动手写一个自己的算法前无意找到了 [menehune23/projection](https://github.com/menehune23/projection)，一个非常不错的封装但只有4个star。

简单试了一下发现十分靠谱，**[改了个他的 demo 画出了一个正立方体](https://m.okjike.com/originalPosts/5c90ed1a7c50db00106947ad?username=F65E8535-A827-4B75-8583-EFD4EB5C2671)**。

作者还提供了转换代码，可以把 `.obj` 的模型轻松转成程序要求的数据格式（虽然这个转换工具其实并不能真的工作，后面会详细展开）。想到之后随意换模型也不难，于是放下没再继续折腾。

## 体感操作

放了一些时间，突然想起我买的是个带 [MPU9250](https://www.invensense.com/products/motion-tracking/9-axis/mpu-9250/) 的版本，Google 了一番发现这个芯片功能也非常强大，如官网所说 gyro + accelerometer + compass 一应俱全（陀螺仪、加速计、磁感应）。那么最简单的，做个体感操作 3D 模型的功能如何。

又是经过一番研究，在 [M5Stack](https://github.com/m5stack/M5Stack/blob/master/src/utility/MPU9250.h) 官方库里找到了一个 MPU9250 的封装，虽然 M5Stick（似乎）不能直接用 M5Stack 的库，但这个封装一样可以用。参考官方的 [example](https://github.com/m5stack/M5Stack/blob/master/examples/Modules/MPU9250/MPU9250BasicAHRS/MPU9250BasicAHRS.ino)，用起来并不是难事，不用去了解太多通信的实现。

给之前的程序加上用加速计（绝对位置）来计算用户在各个轴移动的差值，以此作为转动模型的速度。于是一个**[用倾斜控制模型转动的功能](https://m.okjike.com/originalPosts/5c91ed5c66dda10010d29ebf?username=F65E8535-A827-4B75-8583-EFD4EB5C2671)**就完成了。还发了[推](https://twitter.com/Arthraim/status/1108351962934722560)，官方也来点赞了，高兴。

## 模型转换

有了模型显示和操作，就想要看看换别的模型玩。虽然在即刻上喊了几句希望有人给个模型，不过拿来主义的阴谋最终没有得逞。鉴于我 CGJ18 之前修得一身 C4D 简单建模的功夫，加之当时还真是做了一个[即刻图标的 3D
 版本](https://www.instagram.com/p/Bk8EsZ_FufJ/)，于是轻松撸了一个 3D 的 J 字，多边形不多，适合我们的小硬件。
 
正如前面提到的，转换的时候出了问题，原作者提供的[转换代码](https://github.com/menehune23/projection/blob/master/Projection/ObjConverter.html)只转出了「顶点」，却没有「边」的信息，转出来的是信息都是 `nan nan`。看着情形是转换出了计算问题，不得已开始看他的代码。查了 Wikipedia 的 [Wavefront .obj file
](https://en.wikipedia.org/wiki/Wavefront_.obj_file) 词条发现 C4D export 的 .obj 并没有 `l v1 v2 ...` 的信息，而是 `f v1/vt1/vn1 ...` 这样的格式，原作者在 `f` 的处理犯了错。改正后提了[一个 PR](https://github.com/menehune23/projection/pull/1)，可惜作者似乎是个电子行业的开发，远离 github 已久，想继续玩下去的朋友就暂时用[我的 fork](https://github.com/Arthraim/projection) 吧。

之后就成功**[把即刻模型显示到了小小的屏幕上](https://m.okjike.com/originalPosts/5c923624f7b0dc00119bff68?username=F65E8535-A827-4B75-8583-EFD4EB5C2671)**，帧数还挺高。

## 开源代码

没什么动力继续做些什么了，于是把代码也放到了 [Github](https://github.com/Arthraim/StickGyroCube) 上。

其实理论上这个硬件还可以链接网络，下载 `.obj` 文件。而且 `.obj` 转换其实用 C 语言来写也不难，projection 的作者好像也有一个简单的 C 的实现。所以其实完全可以做个从网上下载模型，然后在板上完成转换直接渲染的功能。

另外我没有加 deep sleep 的代码。所以每次都会跑到没电为止。

有兴趣的同学不妨继续玩下去撒。

