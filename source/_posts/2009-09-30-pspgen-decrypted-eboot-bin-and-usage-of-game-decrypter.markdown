---
comments: true
date: 2009-09-30 11:13:38
layout: post
slug: pspgen-decrypted-eboot-bin-and-usage-of-game-decrypter
title: PSPgen小组破译EBOOT.bin及Game Decrypter使用方法
wordpress_id: 112
categories:
- Video Game
tags:
- eboot.bin
- PSP
- PSPgen
---

PSPgen小组如约发出了5.55的eboot的破译程序，经过我刚才的测试已经顺利的让GT PSP跑在我的PSP上了。我下载的GT是欧版的，我的PSP是1000型并且使用的是5.50GEN-B(FULL)。这里我会讲解一下这次破解的原理，几乎和PSP的型号是没有关系的，只是我现在还没有机会在3000上试验，据一些论坛的反馈，在3000上还是存在一些问题。好了现在展开说一下吧。




**一、原理讲解**




众所周知，dark_alex已经几乎弹出了破解界，据他自己所说hacker needs to eat，当然说他深藏不露、蓄势待发的消息不绝于耳。因为PSP新的CPU对ipl加载等方面做了很多的防御措施，所以到目前为止无法进入kernel mode，也就是核心模式，即我们还是只能在用户模式下徘徊。




之前利用chickhen，可以成功的破解TA-088v3以及PSP3000的TA-090主板又是什么原理呢？这和当初破解2.71时一样，利用的是一个TIFF的漏洞，可以利用它跳转执行自己的代码。TIFF模块不是什么深奥的东西，就是来显示tiff图片的，因此我们才看到了chickhen的破解第一步是要在图片目录里折腾，这就是引发漏洞，只有引发了这个漏洞才能执行我们的代码，完成破解。




而今番再次鼓吹告破，其实不是什么和以上有关的内容，因为上面的思路都是制作自己的自治系统，从而可以轻易的使用索尼的所有模块而不需要去了解这些模块是如何工作的。而这一次的思路完全不同，是关注每个游戏，也就是说，要让每一个高于5.50系统（目前为止版本最高的自制系统）的游戏都可以在5.50上甚至更低级的系统上跑。




那么修改游戏有哪些方法呢，困难又在哪里呢？




**1、使用替换的version.txt**，就像Arthur在《[破解PSP 5.55系统的雷人Lua代码](http://arthraim.cn/post/2009/09/109.html)》里说的一样，替换一个version.txt就能骗过很多对系统有要求的demo甚至游戏。




**2、用boot.bin替换eboot.bin**，如果你用Winrar解压缩过PSP的ISO文件，那么其实你会发现存在eboot.bin和boot.bin两个用以启动游戏的文件。以前的做法是，直接把boot.bin改名为eboot.bin并且替换原来的文件，打包。就可以运行了。




但是这样的做法在现在的版本中（5.55之后）不行了，因为索尼防御了这个做法，如果用HEX工具打开boot.bin来开的话，可以看到全是000000，被成为dummy file。事实上索尼已经不再需要这个文件来加载游戏了。而以前的boot.ini往往是以~ELF开始的。同时，eboot.bin也被加密了，无法按照正常的HEX修改方法去修改它对版本的需求，需要decrypt才可以。




**3、利用版本间的差异**，这也是一个因为无法破译新文件而产生的另类的方法，运气很好的是有些游戏（比如灵魂能力）虽然一些版本（比如日版）的eboot.bin是加密的，但是其他版本（比如欧版美版）没有加密，所以只要做个迁移的工作，就可以搞定了。




**4、破解eboot.bin**，这是在破解每个游戏上比较治本的方法了，如果能破译eboot.ini，那就能按照传统的方法去处理了。其实说治本也不见得，因为这毕竟是比较被动的方法，人家加密了，我们解密……




那这一次，PSP gen小组就告诉我们，可以破译eboot.bin了。




* * *







**二、破解方法**




1、用UMDGEN4.0打开GT的ISO。




2、解压出eboot.ini




[![](/upload/2009-09-30_excompress_eboot_bin.jpg)](/upload/2009-09-30_excompress_eboot_bin.jpg)




3、把解压出来的EBOOT.bin拷贝到PSP记忆棒根目录下




4、在PSP上运行使用Game Decrypter Yoshihiro




[![](/upload/screenshot_9930112319_811.png)](/upload/screenshot_9930112319_811.png)




[![](/upload/2009-09-30_run_game_decrypter.jpg)](/upload/2009-09-30_run_game_decrypter.jpg)




5、PSP记忆棒根目录会生成一个DECRYPTOR目录，取出里面新的eboot.bin




[![](/upload/2009-09-30_get_new_eboot_bin.jpg)](/upload/2009-09-30_get_new_eboot_bin.jpg)




6、用UMDGEN替换原有文件




[![](/upload/2009-09-30_replace_eboot_bin.jpg)](/upload/2009-09-30_replace_eboot_bin.jpg)




7、最后保存为新的iso文件




[![](/upload/2009-09-30_save_new_iso.jpg)](/upload/2009-09-30_save_new_iso.jpg)




[![](/upload/2009-09-30_saving_new_iso.jpg)](/upload/2009-09-30_saving_new_iso.jpg)




这样，新的ISO就产生了，赶快装进PSP试试看能不能用吧~




[![](/upload/screenshot_993011372_491.png)](/upload/screenshot_993011372_491.png)




[![](/upload/UCUS-98632_9930124135_098.png)](/upload/UCUS-98632_9930124135_098.png)







**相关下载**：[Game Decryptor](ftp://download1:fmMRHK15@ftp2.tg777.com/PSP/tools/gamedecrypter-by-yoshihiro.zip) | [UMDGEN4.0](ftp://download3:fmMRHK15@ftp2.tg777.com/PSP/tools/UMDGen4.0.rar)




**相关链接**：[PSP GEN官方发布页](http://www.pspgen.com/tutoriel-game-decrypter-yoshihiro-article-190008-1.html) | [cngba论坛教程](http://www.cngba.com/thread-18457527-1-1.html)




以上……



