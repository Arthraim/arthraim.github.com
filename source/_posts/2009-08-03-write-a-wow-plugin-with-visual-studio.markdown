---
comments: true
date: 2009-08-03 20:50:34
layout: post
slug: write-a-wow-plugin-with-visual-studio
title: 用最习惯的Visual Studio编写魔兽世界插件
wordpress_id: 89
categories:
- Programing
tags:
- Addon
- Lua
- Visual Studio
- WOW
---

Arthur（据说现在独立博客为了防止被非法转载都这么干）相信熟悉.net 的朋友也一定熟悉Microsoft Visual Studio系列的产品。当然，哪怕是你只是在学习C语言时候，经历了Visual C++ 6.0时代，也有理由相信你会习惯之后的Visual Studio系列。Arthur这一次要介绍的，就是用类似Visual Studio来开发魔兽世界（World O Warcraft）的插件，而要用到的，是一个简单的开源项目，它让你根本不需要安装Visual Studio系列产品也可以完成开发。




[     ![](/upload/AddOnStudioBanner.jpg)](/upload/AddOnStudioBanner.jpg)




**AddOn Sudio For World of Warcraft** ---- 这就是今天要介绍的主角了。




一直相信Visual studio的外围扩展能力是很强的，定制的插件、工具都是很强的。看见这个开源项目之后，足以让我相信它真正的强大了。这是一款完全独立于任何一个Visual Studio产品存在的IDE，只是用的都是VS的库。这个IDE本身集成的是Lua的解释器。拥有一个project template就是魔兽世界的插件工程。




具体关于它的其他特性以及下载地址请看[这里](http://addonstudio.codeplex.com/)。




在这里Arthur简单的写一个例子，稍微写下步骤，没什么代码，看图就好，轻松愉快。




**一、环境配置**




下载完成安装包之后安装。如果安装失败，请先下载[Visual Studio Shell](http://mschnlnine.vo.llnwd.net/d1/coding4fun/AddOnStudio/VSShell90SP1/vs_shell_isolated.enu.exe)，再重新安装。安装之前一样要确认有装魔兽世界。但是安装它是不需要安装Visual Studio的。







**二、编写代码、设计界面**




1、打开程序，看到以下界面。无须Arthur多介绍，整个一个Visual Studio，只是标题不同而已。（博客使用Lightbox，图片点击放大）




[     ![](/upload/2009-08-03_AddonForWOW.JPG)](/upload/2009-08-03_AddonForWOW.JPG)     [     ![](/upload/2009-08-03_AddonForWOW_UI.JPG)](/upload/2009-08-03_AddonForWOW_UI.JPG)




2、然后看看究竟区别在哪里。新建一个工程试试就可以看到如下界面，果然不但有魔兽的插件工程，而且只有魔兽的插件工程。很好，我很喜欢这种内聚的感觉。




[     ![](/upload/2009-08-03_AddonForWOW_NewProject.JPG)](/upload/2009-08-03_AddonForWOW_NewProject.JPG)




3、用可视化的设计器来设计一个窗口吧～ 然后写一个按钮的事件，打印Hello, world。 （没有任何魔兽世界插件开发的知识，所以我就划水一下简单点好了）




[     ![](/upload/2009-08-03_AddonForWOW_AllProjectInterface.JPG)](/upload/2009-08-03_AddonForWOW_AllProjectInterface.JPG)




4、按运行后就会自动弹出魔兽世界应用，然后输入帐号密码登录吧，你写的插件已经自动开启了。看看Arthur的程序，hello,world而已。（对了，玩魔兽世界我只是个Light User，很少玩，所以不要尝试从截图里看出我的游戏情况，嘿嘿）




[     ![](/upload/2009-08-03_AddonForWOW_Runtime.JPG)](/upload/2009-08-03_AddonForWOW_Runtime.JPG)







**三、特殊注意点**




这里Arthur也遇上了一些小问题，所以写出来分享下。就是编译后的插件在游戏中显示"过期"，一下子就想到了版本问题。再回头去看工程里，有一个"工程名.toc" 的文件，熟悉魔兽世界插件开发的朋友一定比Arthur还要清楚，这就是插件的版本修改要修改的文件，既然还在开发阶段，那么我就可以随便修改啦～ 把interface后面的版本号改成30100，就可以在国服客户端里正常运行了。（虽然国服是3.13了，不过我看其他的插件版本一律都是30100，并且Arthur尝试过31300是不可以的，所以权且这样写吧。）




后来发现右键工程属性，也有可视化修改这些信息的地方。和其他VS工程一样方便。




[     ![](/upload/2009-08-03_AddonForWOW_Property.JPG)](/upload/2009-08-03_AddonForWOW_Property.JPG)







偶然发现这个开源项目，所以就当是推荐开源项目好了。
