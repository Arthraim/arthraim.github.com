---
comments: true
date: 2009-06-13 13:24:27
layout: post
slug: xna-tutorial-2d-ch3-add-assets
title: XNA教程-2D游戏-03-添加Assets
wordpress_id: 63
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---

[![](/images/uploads/zb/2009-06-13_WhatIsAssets.jpg)](/images/uploads/zb/2009-06-13_WhatIsAssets.jpg)




**XNA教程 -- 2D游戏 -- Chapter 03 -- 添加Asset**




这一部分我们将完成的工作是在工程中添加Asset，听上去好像很复杂，Asset是什么？添加到工程又是什么概念？那其实是很简单的事情，我会通过三个部分的解说包括实际的操作方法来说明这次的章节，看不懂的可以看视频的，那么接下去就正式开始写了。




**一、Visual Studio的用法。**




这就是我说的三个部分中的第一个，VS的解说。肯定很多看到这里的人要不耐烦了，你弄这种东西干什么啊，都来看XNA了当然会C#会.net会VS啊。其实，之所以加上这一块，是因为我惨痛的童年回忆造成的。很多向往做游戏的儿童青少年们，好不容易找到了一个教程，可是往往教程默认了很多很多的前提以至于一头雾水。那么看到这里的小朋友们～ 记住，我也默认了很多前提在这里，你们可以先去学C#，用C#做一个按一下按钮出来一个对话框显示一个Hello,world的，OK基本上你就可以来看这里的内容了（囧，可以吗？）好讲一下VS。VS有solution（解决方案）的概念，这是比project（项目）更加高级的东西。一个solution中可以有多个project，很常见的就是dll一个界面一个。VS提供了一些控件，比如类视图、解决方案视图、控制台、追踪、错误列表等等，这些和普通的IDE都是一样的。（写到这里自己都觉得多余了）




**二、什么是Asset？**




说完多余的东西，我们终于可以将一些重点了。究竟Asset是什么呢？我觉得程序里涉及到一些哪怕日常的单词也不应该去翻译，比如并行设计到的actor模型，难道翻译成角色模型？不合适吧。所以Asset如果翻译成"资产、有用的东西"之类的意思，那也就真的不合适了。所以还是用asset原来的样子来讲解比较好，通过后面的讲解你一定很快明白什么是asset了，微软忽悠这么一个概念也挺强大的。




（注：以下文字由本人翻译自微软官方解释。）




在XNA Game Studio项目中，你会频繁的听到"代码"和"asset"这样的措辞。"代码"指在程序说明中包含的.cs文件。这些说明告诉游戏怎么行动：怎么对用户操作进行反应，怎么移动物体，以及怎么显示给玩家看。




另一方面的asset，不是唯一的一个文件类型。位图图形、声音文件、3D模型 ---- 他们都是asset。总之，accets指数据（图形、声音或其他一些东西），他们被"代码"操控着。举个UFO图片的例子。这个UFO图片本身，存储为.tga或者.png文件，他是一个asset。代码可能载入这个UFO图像，并且把它显示在屏幕的不同位置，并给他一些移动的效果。如果有一组asset的话，它不被称为content。




在游戏中，你必须同时拥有"代码"和"asset"。典型的，asset表现出物体的外表和声音，你的代码代表了它的动作（行动）。




这个规则有一个例外，就是一个asset的特殊类型，叫做shader。他是一些特别的代码，帮助绘制图形在屏幕上，但是它不是图片。Shader不会在这次的教程中使用到，但是在使用高级图形的游戏中它们经常被用到，并且要像代码一样的进行编辑就。




在教程中，我们只使用两种类型的asset ---- 2D texture（材质，还是不要翻译了）和font（字体）。2D texture只是一些图片文件。在教程中，我们每个游戏元素拥有一个图片 ---- 我们玩家的加农炮，敌人的UFO，玩家可以发射的加农炮的子弹，还有一个背景。font字体是特殊的一种texture，代表了我们可以画在屏幕上的文本。它将在游戏中当我们需要跟踪玩家得分时帮助到我们。




**三、如何添加？**




步骤其实非常的简单，在工程中我们可以看到Contend的存在。当前里面只有一个引用项，不去管它。第一步我们要做的是下载我们这次要用到的素材，或者现在应该叫做asset。（直接看到此文的请转至Chapter2补下，当然你也可以在本文最后的完整工程代码下载中找到你要的文件）




下载下来的是一个BG_2D_Chapter2.Zip文件，解压缩后得到Microsoft Permissive License.rtf和Sprites文件夹。Sprites文件夹里有之前说到的4张图，background.tga, cannon.tga , cannonball.tga, enemy.tga。




要加入到工程中很简单，两步走。




1、右键单击Content -- Add -- New Folder。然后命名为Sprites。2D游戏里都这么称呼，那么我们就这么命名。（中文版自重）




2、右键单击Sprites文件夹 -- Add -- Existing Items。然后选中你刚才解压缩出来的四个.tga文件，add就好了。（其实你愿意的话，把解压缩出来的文件夹复制到工程里也行，只是一样要add过而已）




[![](/images/uploads/zb/2009-06-13_AddAssets.jpg)](/images/uploads/zb/2009-06-13_AddAssets.jpg)




很神奇吧，做完这个，其实内容就已经完成了，看上去真是有点脑残了。完成如下图。




[![](/images/uploads/zb/2009-06-13_AddedAssets.jpg)](/images/uploads/zb/2009-06-13_AddedAssets.jpg)




这就是本章要讲解的内容，万事开头难，大家要有耐心，虽然做起来很简单，不过无论是Content还是Asset都是微软给我们的概念，一定要记住了。




[![](/images/uploads/zb/2009-06-12_download_XNA.png)示例代码下载](http://creators.xna.com/downloads/?id=150)
