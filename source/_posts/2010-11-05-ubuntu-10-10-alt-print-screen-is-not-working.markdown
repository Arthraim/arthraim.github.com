---
comments: true
date: 2010-11-05 14:21:05
layout: post
slug: ubuntu-10-10-alt-print-screen-is-not-working
title: Ubuntu 10.10的Alt+Print Screen截图无效
wordpress_id: 637
categories:
- O.S.
tags:
- Linux
- Ubuntu
---

升级到10.10之后Alt + Print Screen就失效了, 今天Google了下发现原来不单单是我的问题, 这里有个解决办法. 在terminal输入以下命令:



    
    sysctl -w kernel.sysrq=0




就搞定了. 如果重启之后问题依旧就把这一行加到 /etc/rc.local 里. 不知道这个问题会不会得到ubuntu之后升级的修复.




Good luck.




参考:





	
  * [Alt+Print Screen not working (Ubuntu 10.10)](http://www.virtualhelp.me/linux/212-altprint-screen-not-working-ubuntu-1010)

	
  * [Magic SysRq key](http://en.wikipedia.org/wiki/Reisub)


