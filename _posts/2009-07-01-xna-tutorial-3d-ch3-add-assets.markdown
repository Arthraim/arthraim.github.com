---
comments: true
date: 2009-07-01 16:17:14
layout: post
slug: xna-tutorial-3d-ch3-add-assets
title: XNA教程-3D游戏-03-添加Asset
wordpress_id: 73
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---

到了这里其实还是和2D的教程是重叠的，这里我们要做的是添加asset们。所以这里你要问自己三个问题：






  1. 你已经建立了工程（project）了吗？---- [点我补课](http://arthraim.cn/post/2009/06/72.html)


  2. 你已经下载了上一部分要求下载的资源了吗？---- [点我下载](http://creators.xna.com/downloads/?id=157)


  3. 你知道什么是asset吗？你看过之前2D教程的添加asset内容吗？---- [点我补课](http://arthraim.cn/post/2009/06/63.html)（重点浏览链接指向的文章的后半部分，讲解什么是asset）




三个问题过关的，再来看我的这里的屁话。放假回家，所以把编译一个XNA游戏都要3分钟的windows xp给格掉了（关注我的twitter的一定知道了），所以把windows 7的引导也格掉了。因为找不到之前安装windows 7的那张光盘，所以一下子没有修复引导，所以这一次的教程就先用XP完成了。装了3、4的系统，终于把一个一个的东西下载下来给装上了，现在可以继续了。




OK言归正传，看看这一次要做的是如何简单。和2D类似的右键添加等等工作，就能完成了。







**第一步、添加models到工程的content中**




Content中的asset可以在content pipeline中被我们调用。这是一个非常方便的设计，当然这也使得我们这一步的工作显得非常重要，你可以从磁盘去读取一些你需要的资源，但是让他在content管道中，会大大方便你的管理。




2D教程中，我们加入一些.tga资源到Content/Sprites中，我们右键新建文件夹，然后添加存在的资源等等这样完成了这部工作，那么这一次和视频一样，我们偷懒一下。直接把解压缩出来的models文件夹拖到VS2008的树状Solution Explorer的Content目录上，来完成这一步工作。




添加进来之后，我们看到了一些.fbx文件和.tga文件，fbx就是我们的模型，而tga是这些模型对应的贴图，不会被我们实际的工作主动调用到，他们只要在他们应该在的磁盘位置上就可以了，在我们调用模型的时候，相关联的贴图会自动被寻找，所以把他们排除我们的工程吧。右键选择所有的.tga文件，选择"Exclude From Project"将其排除工程。




[![](/images/uploads/zb/2009-07-01_ExcludeTgas.JPG)](/images/uploads/zb/2009-07-01_ExcludeTgas.JPG)







**第二步、添加audio到工程的content中**




这还分成两步看起来有点傻，一样的，把解压缩出来的audio文件夹也一并拖到Solution Explorer的Content目录上。展开Audio文件夹，右键点击Waves目录，选择"Exlude From Project"，把这些.wav文件全部排除在工程之内。因为对于声音的asset处理会有很大的不同，所以这里我们暂时先这样处理就好。




准备就绪之后，就可以开始代码阶段的工作了。







【官方工程下载】




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=158)
