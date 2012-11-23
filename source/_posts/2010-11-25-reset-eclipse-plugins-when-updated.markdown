---
comments: true
date: 2010-11-25 14:49:12
layout: post
slug: reset-eclipse-plugins-when-updated
title: 升级造成eclipse插件失效
wordpress_id: 666
categories:
- Programing
tags:
- eclipse
---

每次发生这样的问题都要重新Google, 每次不外乎就是那么几种方法, 记录下, 自己找起来方便一点:






	
  * %eclispe_dir%/configration/config.ini文件
		
			
    * org.eclipse.update.reconcile=false, 改成true

			
    * 或者加上osgi.checkconfiguration=true

		
	

	
  * 删除整个目录/eclipse/configuration/org.eclipse.update/，重启eclipse

	
  * 在启动eclipse时带上 -clean参数




记录之, 以上.
