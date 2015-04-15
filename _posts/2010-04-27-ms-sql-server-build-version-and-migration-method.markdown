---
comments: true
date: 2010-04-27 15:05:28
layout: post
slug: ms-sql-server-build-version-and-migration-method
title: SQL Server各版本编号及迁移方法
wordpress_id: 339
categories:
- Programing
tags:
- build
- Microsoft
- SQLServer
- version
---

使用sql server附加数据库的时候会遭遇到一些错误，其中有一个很常见的就是"数据库XXX的版本为655，无法打开。此服务器支持643版及更低版本。不支持降级路径。"其实是很困扰的错误，因为你明明知道你的版本低，可是不知道自己应该装什么版本。比如我是把一个10.0.2531上的数据库attach到10.0.1300的数据库上发生了上面的错误。




于是Google了一下，找到了[这个文章](http://sqlserverbuilds.blogspot.com/)(墙外)的那些东东，就是我们解决这个错误的最好办法。查一下就可以知道10..0.2531就是SP1，那么装上SP1就好了吧~




当然要把数据库移到旧版本的需求肯定是有的，那要怎么做呢？首先可以确定的是"附加"肯定是不行，另外"备份-还原"也是不行。[这里](http://social.microsoft.com/Forums/zh-CN/sqlserverzhchs/thread/aa561da8-db83-4304-84f2-8c4995b95994)有个好办法：





	
  1. 在2008中右键点击数据库 - 任务 - 生成脚本。

	
  2. 在向导中为整个库的对象生成脚本，并设置好相关的脚本生成选项，脚本的服务器版本要选择你需要的低版本，比如sql server 2005。

	
  3. 去低版本（如2005）中执行刚才生成的脚本，数据结构就好了。

	
  4. 最后再用数据导入/导出向导把数据导过去




今天把某些数据给同学的时候发生的错误，因为对方的版本低一点，记录一下。




**Update：**




比较郁闷的是我同学的10.0.1300是可恶的CTP版本，这个版本不能直接安装SP1，非常郁闷，而SP1最起码装在10.0.1600也就是RTM版上，这真是有些郁闷的。




以上。
