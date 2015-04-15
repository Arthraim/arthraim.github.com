---
comments: true
date: 2009-10-04 22:54:21
layout: post
slug: prxdecrypter-2-3-released-6-10-version-txt-and-pspgo-got-helloworld
title: PRXdecrypter 2.3发布、6.10系统的version.txt、PSPGO运行hello,world
wordpress_id: 115
categories:
- Video Game
tags:
- 破解
- eboot.bin
- Hello World
- prx
- PSP
---


这里更新一些关于PSP破解的新消息。一个，就像我之前所说，依旧要关注prxdecrypter等破解情况，不过今后除了重大更新，其他就不再发到博客上了，会在人间网和twitter上推更新。关心的玩家朋友破解朋友们也可以自己去文章末尾的论坛链接关注更新进程。另外还有6.10版本的version.txt文件，及PSPGO破解进程。

引用如下：

> VERSION 2.3 DOES NOT NEED prxdecrypter_03g.prx
> 2.3版本不再需要prxdecrypter_03g.prx这个文件
>
>
> Quote:
> 2.25 --> 2.3
> - "Analyze files" option added to menu - displays info about the files without changing them
> - User module keys up to 6.00, not sure if they're the ones used in the firmware though!
> - Fixed decryption issue - extra data added to decrypted files
> - Rewritten kernel modules for 2.XX+ allowing fewer external files and cleaner code
> - Rewritten handling of compressions - 1.50 has RLZ with the correct file, 2.71 to 3.80 have RLZ, 3.80+ has KL3E, KL4E and may have RLZ depending on the exact firmware version
>
> 2.1 --> 2.25
> - 2.2 - limited release, 5.00 keys
> - 2.25 - EBOOT.BIN keys up to 6.00! (Many thanks to an anonymous friend)
> There is another odd one not present in either decrypter: 0xD91612F0 (if you get an error message with this code, post here! I need to work out where this tag is being used. EDIT: Gran Turismo from PSN seems to be one of them)
>
> To anyone on PSPGEN who accused Yoshihiro of copying/stealing my work: this is complete nonsense.
>
> You should be able to decrypt any of the below EBOOT.BINs (except the last one) even on 5.00 M33 and below, as long as PRXdecrypter runs.
>
> General EBOOT.BIN keytags info
> The classics:
> Tag 0x08000000 - 1.xx EBOOT.BIN
> Tag 0xC0CB167C - 2.xx EBOOT.BIN
>
> Unknown if these were ever used, but I found them ages ago - Sony were planning new EBOOT keys for a long time:
> Tag 0x8004FD03 - 2.71 EBOOT.BIN
> Tag 0xD91605F0 - 2.8X EBOOT.BIN
> Tag 0xD91606F0 - 3.0X EBOOT.BIN
> Tag 0xD91607F0 - ???
> Tag 0xD91608F0 - 3.1X EBOOT.BIN
>
> These are now being used:
> Tag 0xD91609F0 - 5.00 EBOOT.BIN
> Tag 0xD9160AF0 - 5.50 EBOOT.BIN
> Tag 0xD9160BF0 - 5.55 EBOOT.BIN
> Tag 0xD9160CF0 - 6.00 EBOOT.BIN (which game(s) use this?)
> Tag 0xD91611F0 - 6.00 EBOOT.BIN (which game(s) use this?)
> Tag 0xD91612F0 - 6.00 EBOOT.BIN (GTN USA - PSN edition, any others?) not decrypted!

原帖地址：[http://forums.maxconsole.net/showthread.php?t=143538](http://forums.maxconsole.net/showthread.php?t=143538)

**************

另外是6.10的version.txt文件，只要使用《破解PSP 5.55系统的雷人Lua代码》里的Lua代码一样可以把version.txt写入flash0中达到一定的欺骗目的。

```
release:6.10:
build:3745,0,3,1,0:builder@vsh-build6
system:54865@release_610,0x06010010:
vsh:p6501@release_610,v55286@release_610,20090918:
target:1:WorldWide
```

**************

值得一提的是已经有一种或两种无法被证实是否属实的方法让PSPGO跑起hello world了，PSPGO能否利用内部闪存完成ISO读取的破解还是个悬念，并且从目前的情况来看破解工作依然停留在user-mode。

找到了Freeplay小组发布的视频的youku版本~ （freepaly就是早前利用GridShift存档内存漏洞加载自己代码的小组）

<p><embed src="http://player.youku.com/player.php/sid/XMTIzMDY4NTc2/v.swf" quality="high" width="480" height="400" align="middle" allowscriptaccess="sameDomain" type="application/x-shockwave-flash"></embed></p>

以上……