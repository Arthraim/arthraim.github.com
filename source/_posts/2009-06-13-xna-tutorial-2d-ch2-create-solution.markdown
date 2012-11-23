---
comments: true
date: 2009-06-13 12:47:02
layout: post
slug: xna-tutorial-2d-ch2-create-solution
title: XNA教程-2D游戏-02-创建游戏工程
wordpress_id: 62
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---

[![](/upload/2009-06-13_PlatformerGameDemo.jpg)](/upload/2009-06-13_PlatformerGameDemo.jpg)




** XNA教程 -- 2D游戏 -- Chapter 2 -- 创建游戏工程**







从最开始学习XNA，那么首先我们需要自己的环境。微软提供XNA Game Studio给大家，当然还要有Visual Studio，如果愿意你可以在安装好Visual Studio 2005或更高版本的前提下安装XNA Game Studio（以下简称GS）。当然，本着XNA游戏开发是基本免费的原则，我们也可以使用Express版本，也是支持的。具体VS和GS的安装和配置请大家自己解决了，这里不赘述了。




使用GS3.1+VS2008是基于.NET Framework 3.5的，否则一些新特性将不会被用到，如果你的开发使用其他环境，那么也许会不一样。







**步骤一：Visual Studio简单介绍**







这一步骤简单的雷人。稍微讲讲就过去了吧。首先要安装好VS和GS，就像之前说到的，然后我们看一下VS的新建。新建中总共有这么几项（GS3.1+VS2008，不保证其他版本有一样的工程）




[![](/upload/2009-06-13_VS2008GS31.jpg)](/upload/2009-06-13_VS2008GS31.jpg)




> 

> 
> 

>   * Windows Game (3.1) ---- XNA Game For Windows游戏的开发，很多游戏打着这个标志
> 

>   * Windows Game Library (3.1) ---- Windows游戏类库
> 

>   * Xbox 360 Game (3.1) ---- Xbox360游戏
> 

>   * Xbox 360 Game Library (3.1) ---- Xbox360游戏类库
> 

>   * Zune Game (3.1) ---- Zune游戏
> 

>   * Zune Game Library (3.1) ---- Zune游戏类库
> 

>   * Content Pipeline Extension Library (3.1) ---- Content管道扩展库
> 

>   * Platformer Starter Kit (3.1) ---- 一个游戏示例（提供windows x360 zune三种版本都有，本章顶部图为Windows版）
> 






VS的具体操作就不介绍了，至于最终我们的游戏如何使用，如果你没有加入XNA Creator Club成为白金会员，那么你的游戏也是没办法在XBOX360上用的，我这里就简单的使用Windows开发了。另外，视频中使用的是VS2005+GS2，所以我们直接可以看到两个视频中制作的游戏的示例工程，偷懒的直接下2吧。究竟3有什么新特性，我也不是很了解，也许讲到最后我也就了解了吧。（总觉得自己都不会就开始写是非常不妥的方法。）







**步骤二：新建工程，获取资源**







这一步也非常的简单，根据刚才介绍的新建，建立相应的工程。




比如我新建一个Windows Game (3.1)工程，并且取名为Chapter2。




![](/upload/2009-06-13_Project.jpg)




可以看到IDE为我们创建了如图一些文件，具体都是什么呢？




> 

> 
> 

>   * Content ---- 就是一切我们需要的游戏资源，在XNA中存在Content Pipeline这个概念，而这里的所有资源可以之后在Content管道中直接调用加载。
> 

>   * Game1.cs ---- 游戏逻辑代码。
> 

>   * Game.ico ---- 不用多说，一个Xbox360的手柄样式的ico图标。
> 

>   * GameThumbnail.png ---- 正如他的名字，是一个缩略图，当然要你自己画的啦。
> 

>   * Program.cs ---- 就是程序的入口Main方法了。
> 






GameThumbnail.png原图如下  

![](/upload/2009-06-13_GameThumb.jpg)




接下去本章很重要的一部就是下载必须的游戏素材。这里提供官方链接，下载解压缩后放入Content文件夹中，这是我们制作我们示例要用到的全部素材了。只有一个lisense文件和sprite文件夹，sprite文件夹中有四张图，看看就知道干什么了～




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=149)素材下载点击图标




OK，好了这一章很简单，就说到这里吧。



