---
comments: true
date: 2009-10-02 19:46:07
layout: post
slug: psp-hacking-awake-jas0nuk-released-prxdecrypter-2-25
title: PSP破解复苏 jas0nuk发布PRXdecrypter 2.25
wordpress_id: 114
categories:
- Programing
tags:
- eboot.bin
- prx
- PSP
---

Arthur在[上一篇](http://arthraim.cn/post/2009/09/112.html)文章中说到过jas0nuk已经回归the scene（PSP破解界），而今天早些时候他就如约发布了PRXdecrypter的新版本2.25。可以说这次的版本功能和Yoshihiro的Game Decrypter是一样的，用以解密eboot.bin，只是包含了所有的Eboot.bin的密匙，即你可以解密目前为止所有的eboot.bin了。务须多说，这里是官方说明。




>

>
> ![](/images/uploads/zb/bankey.gif)
>
>

>
> Contains all the 6.00/6.10 EBOOT.BIN keys, just like those in Yoshihiro's decrypter.
>
>

>
> There is another odd one not present in either decrypter: 0xD91612F0 (if you get an error message with this code, post here! I need to work out where this tag is being used.)
>
>

>
> To anyone on PSPGEN who accused Yoshihiro of copying/stealing my work: this is complete nonsense.
>
>

>
> You should be able to decrypt any of the below EBOOT.BINs (except the last one) even on 5.00 M33 and below, as long as PRXdecrypter runs.
>
>

>
> General EBOOT.BIN keytags info

The classics:

Tag 0x08000000 - 1.xx EBOOT.BIN

Tag 0xC0CB167C - 2.xx EBOOT.BIN
>
>

>
> Unknown if these were ever used, but I found them ages ago - Sony were planning new EBOOT keys for a long time:

Tag 0x8004FD03 - 2.71 EBOOT.BIN

Tag 0xD91605F0 - 2.8X EBOOT.BIN

Tag 0xD91606F0 - 3.0X EBOOT.BIN

Tag 0xD91607F0 - ???

Tag 0xD91608F0 - 3.1X EBOOT.BIN
>
>

>
> These are now being used:

Tag 0xD91609F0 - 5.00 EBOOT.BIN

Tag 0xD9160AF0 - 5.50 EBOOT.BIN

Tag 0xD9160BF0 - 5.55 EBOOT.BIN

Tag 0xD9160CF0 - 6.00 EBOOT.BIN (which game(s) use this?)

Tag 0xD91611F0 - 6.00 EBOOT.BIN (which game(s) use this?)

Tag 0xD91612F0 - 6.00 EBOOT.BIN (GTN USA - PSN edition, any others?) not decrypted!
>
>








如果你遇到错误代码0xD91612F0，可以到[这里](http://forums.maxconsole.net/showthread.php?t=143538)注册留言，当然也可以在我的博客留言。另外，可以想见不出多日就会有新的修复版发布，以修复一些这个版本没有解决的问题，Arthur将继续跟踪，敬请关注。




**下载地址**：[官方地址](http://forums.maxconsole.net/attachment.php?attachmentid=24494&d=1254444222) | [rapidshare](http://rapidshare.com/files/287691816/PRXdecrypter_2-25.rar)




以上。
