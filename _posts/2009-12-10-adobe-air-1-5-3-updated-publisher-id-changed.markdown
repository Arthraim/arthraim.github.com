---
comments: true
date: 2009-12-10 11:10:50
layout: post
slug: adobe-air-1-5-3-updated-publisher-id-changed
title: Adobe AIR 1.5.3 更新，publisher ID发生变化
wordpress_id: 124
categories:
- I.T.
tags:
- Adobe
- AIR
---

1.5.3的Adobe AIR SDK的运行时和现在可供下载。此版本包括了Flash Player，安全更新和修正了一些错误的更新版本。发展商发行说明包含重要的信息，所有开发建设AIR应用程序应确保重要信息，包括阅读有关证书续期（参阅前面的博客帖子）以及错误修正。




![](/images/uploads/zb/sidebar-logo2.gif)






  * AIR Team Blog: [Adobe AIR 1.5.3 Now Available](http://blogs.adobe.com/air/2009/12/adobe_air_153_now_available.html)


  * [Release notes for Adobe AIR developers](http://www.adobe.com/support/documentation/en/air/1_5_3/releasenotes_developers.html)


  * [下载 Adobe AIR 1.5.3 Runtime](http://get.adobe.com/air/)


  * [下载 Adobe AIR 1.5.3 SDK](http://www.adobe.com/products/air/tools/sdk/)




对于开发者来说，关于publisher ID的修改，要重点关注一下。




如果您有一个现有的AIR应用程序，那么你应采取下列措施，使其转换到新版本：






  1. 确定您的应用程序的当前publisher ID。在一个已经安装的应用程序中，可以在META-INF/AIR/publisherid找到。


  2. 添加一个<publisherID></publisherID>标记到你的descriptor文件中，拷贝你刚才找到的publisher ID到标记里。


  3. 更新您的应用程序的命名空间1.5.3。




新项目就无视了，详见上面的链接吧。
