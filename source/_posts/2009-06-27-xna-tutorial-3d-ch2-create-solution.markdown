---
comments: true
date: 2009-06-27 02:08:42
layout: post
slug: xna-tutorial-3d-ch2-create-solution
title: XNA教程-3D游戏-02-创建游戏工程
wordpress_id: 72
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---

进行了之前的工作，搭建了环境之后，现在就要进入实际开发过程了。我想过程和2D也许存在着一些区别，但是还是相对比较依赖2D教程的，尤其像这一章和3D的确是别无二致，不过我这里还是按照步骤说一下。（参考：[《XNA教程-2D游戏-Chapter2-创建游戏工程》](http://arthraim.cn/post/2009/06/62.html)）




使用GS3.1+VS2008是基于.NET Framework 3.5的，否则一些新特性将不会被用到，如果你的开发使用其他环境，那么也许会不一样。




具体涉及到一些可能和2D教程重复的东西，看过2D的直接跳过就好。







**第一步、Visual Studio简单介绍**







安装高于Visual Studio 2005 Express的版本，并且装好XNA Game Studio（2.0以上）。那么如果安装成功的话，在VS的新建对话框中应该可以看到这么几项（GS3.1+VS2008，不保证其他版本有一样的工程）




[![](/upload/2009-06-27_NewProject.jpg)](/upload/2009-06-27_NewProject.jpg)




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

>   * Platformer Starter Kit (3.1) ---- 一个游戏示例（提供windows x360 zune三种版本都有）
> 









**第二步、新建工程，获取资源**







根据刚才介绍的新建，建立相应的工程。




比如我新建一个Windows Game (3.1)工程，并且取名为TChapter2，那么我们可以得到一些IDE为我们创建的文件，具体情况如下。




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









**第三步、下载资源**




接下去本章很重要的一部就是下载必须的游戏素材。这里提供官方链接，下载解压缩后就可以看到我们本教程的例子要用到的全部素材了。




[![](/upload/2009-06-27_files1.JPG)](/upload/2009-06-27_files1.JPG) .../Audio/Waves/  

[![](/upload/2009-06-27_files2.JPG)](/upload/2009-06-27_files2.JPG) .../Models/




【点击下载官方素材】




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=157)
