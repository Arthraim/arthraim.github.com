---
comments: true
date: 2009-08-27 06:09:58
layout: post
slug: build-a-s60v5-java-environment-with-netbeans
title: 在NetBeans上搭建S60v5的Java开发环境
wordpress_id: 98
categories:
- Programing
tags:
- 5800XpressMusic
- Hello World
- IDE
- Java
- S60
---

入手5800XpressMusic已经有相当一段时间了，随着N97、5530，以及新鲜的5230的推出，S60v5已经在中低高各个层面铺开了道路，应该说诺基亚给予的投入还是非常多的。之前凭着新鲜感搭建了一个S60v5的开发环境，是Symbian C++，当时在[那篇文章](http://arthraim.cn/post/2009/05/39.html)里粗略的提及了S60v5支持的各种开发方式，而今天边看着电视剧，边再一次搭建了J2ME开发环境。




一样默认为Windows平台下。除了和上次一样，下载一个S60v5的all in one sdk包，另外还要下载包含Java ME开发在内的netbeans（这里Arthur手快下载了一个中文版，不符合习惯不过看着先）。当然这一次就用不到Carbide.C++了。总结一下要做的准备如下：






  * 一台电脑


  * 一台S60v5的终端


  * [Java JDK](http://java.sun.com/javase/downloads/index.jsp)


  * [ActivePerl](http://www.perl.com/download.csp#win32) (strawberry也一样，这里是Win32)


  * [S60v5 SDK](http://www.forum.nokia.com/info/sw.nokia.com/id/ec866fab-4b76-49f6-b5a5-af0631419e9c/S60_All_in_One_SDKs.html) (all in one版本)


  * [netbeans](http://www.netbeans.org/downloads/) (包含Java ME的版本)




安装上面准备的那些东西，不赘述安装过程了，不了解S60v5 SDK安装的还是可以去看[以前那篇文章](http://arthraim.cn/post/2009/05/39.html)。这里值得注意一点，先安装过netbeans的话在安装s60v5 sdk时会自动配置netbeans。另外，选择好eclipse路径也可以在配置好支持Java ME的eclipse上加上S60v5相关的插件，这里也不多说了。




安装妥当之后，我们要在netbeans里做些文章，就能做开发了。




1、打开netbeans，选择工具 | Java平台，出现2对话框。




[![](/upload/2009-08-27_J2ME_s60v5.jpg)](/upload/2009-08-27_J2ME_s60v5.jpg)







2、在此对话框中点击左下角 [添加平台. . .] 按钮，出现3对话框。




[![](/upload/2009-08-27_J2ME_s60v5_2.jpg)](/upload/2009-08-27_J2ME_s60v5_2.jpg)







3、选择Java ME MIDP 平台仿真器，进入下一步。




[![](/upload/2009-08-27_J2ME_s60v5_3.jpg)](/upload/2009-08-27_J2ME_s60v5_3.jpg)







4、在这一步点击 [查找更多 Java ME 平台文件夹] 按钮，（或在按此按钮之前自动）弹出路径选择对话框，选择你S60所安装的路径。netbeans会自动检测合法的平台出现在列表中。（路径搜索前后如下两张图）确保选中新发现的平台后，进入下一步。




[![](/upload/2009-08-27_J2ME_s60v5_4.jpg)](/upload/2009-08-27_J2ME_s60v5_4.jpg) [![](/upload/2009-08-27_J2ME_s60v5_5.jpg)](/upload/2009-08-27_J2ME_s60v5_5.jpg)







5、下图可以看到这一步的效果。做你需要的修改，然后点击完成之后安装就大功告成了。




[![](/upload/2009-08-27_J2ME_s60v5_6.jpg)](/upload/2009-08-27_J2ME_s60v5_6.jpg) [![](/upload/2009-08-27_J2ME_s60v5_7.jpg)](/upload/2009-08-27_J2ME_s60v5_7.jpg)







现在，新建Java ME项目，选择仿真器平台的时候选择刚才添加的就可以开发了。




[![](/upload/2009-08-27_New_project.jpg)](/upload/2009-08-27_New_project.jpg)







测试效果如下。




[![](/upload/2009-08-27_runtime.jpg)](/upload/2009-08-27_runtime.jpg)




以上。
