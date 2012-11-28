---
comments: true
date: 2009-04-24 18:20:55
layout: post
slug: first-glance-of-ubuntu-9-04
title: First Glance of Ubuntu 9.04 —— 乌班图的第一瞥
wordpress_id: 17
categories:
- O.S.
tags:
- Linux
- Ubuntu
---

![](/images/uploads/zb/75C14EDC194AA34DDF56AAAD5D470D31.png)




**以下的英文为最初版本，中文为更新时的翻译，跳过英文吧，只是因为一开始在ubuntu下没有安装中文输入法。**







First time downloaded Ubuntu 9.04, and I strongly recommend you to download the one which fits your computer. Ubuntu officially release 7 versions to download. They're desktop, server, alternate for both i386 and amd64, also there's a dvd version for i386.




第一时间下载了Ubuntu 9.04，我强烈建议你们下载适合自己电脑的版本。Ubuntu官方正式发布了7个版本，它们是分别针对i386和amd64的桌面版、服务器版、定制版。




I installed the English version, so there is no time to update Chinese Language-pack. So I can only type down the article in English, I think I will translate it in sometime later.




我装的是英文版，所以没有时间给我补充中文语言包，所以我（写这篇文章的英文版的时候）只能输入英文，我想我会在晚点翻译。（正是现在）




Here comes the download address:




下面是下载地址：




>

>
> [http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-desktop-i386.iso](http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-desktop-i386.iso)
>
>

>
> [http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-desktop-amd64.iso](http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-desktop-amd64.iso)
>
>

>
> [http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-server-i386.iso](http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-desktop-i386.iso)
>
>

>
> [http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-server-amd64.iso](http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-desktop-amd64.iso)
>
>

>
> [http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-alternate-i386.iso](http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-alternate-i386.iso)
>
>

>
> [http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-alternate-amd64.iso](http://releases.ubuntu.com/releases/.pool/ubuntu-9.04-alternate-amd64.iso)
>
>








Follow me and to see the NEW features:




跟着我看看新特性：




1 -- **Gnome 2.26** as you see.




Gnome 2.26就像你看到的。个人感觉是相应速度变快了，因为后来断网了，所以没有装显卡驱动（我是N卡），究竟多华丽在这次是看不到了。




![](/images/uploads/zb/Screenshot-AbouttheGNOMEDesktop.png)




Look at the screenshot, honestly it looks changed little. I hope it can be better after I installed my video card driver.




看着截图，实话说我觉得变化不大，我希望它在我装了显卡驱动之后会变得好一些。







2 -- **OpenOffice.org 3.0** (What a beautiful Sun logo)




OpenOffice.org 3.0（多漂亮的Sun公司标志）。个人是不用桌面Office软件的，我觉得Google docs已经足够我所有的需求了，它不但轻便而且不用在意文档的最新版本存在了哪个U盘里了，并且它有版本管理，我想桌面办公软件对我来说完全没用，唯一的作用就是在发给需要微软doc（或OpenOffice格式）的人前，存为doc（或OpenOffice格式）前自己审核一下格式是不是好看。




![](/images/uploads/zb/Screenshot-AboutOpenOffice.org.png)







3 -- **Speed**. It was said we'll have a fantastic computer starting time, so what's mine. I'll count the seconds after grub menu. (Wait for my update please...)




速度。据说我们会有一个惊艳的开机速度，那么我的呢，我会在grub菜单之后开始数秒。（请等我的更新）




![](/images/uploads/zb/2009-04-24_Ubuntu-startingtime.jpg)




还是注意看第一个停下的时间比较好，16.92，是grub引导到硬盘之后，也就是开机的进度条出现之后，到出现输入Username的登录界面，时间是16.92。网上有人说只有14秒，那也差不多吧，嘿嘿，比较激动，以为没停下来多按了几下。另外，之前最慢的是登陆之后到界面完全出现的速度，不过这一次却是快的一塌糊涂。几乎没有什么感觉了。




4 -- Version. We have server version, desktop version(like mine), and Notebook Remix, it's fasion or something...(poor English)




版本。我们还可以拥有服务器版本，桌面版，还有一个上网本的版本，这是顺应潮流之类的吧。




5 -- 中文补充一个新特性吧，就是ext4文件系统。




在分区的时候会要求选择文件系统，所以这次安装完全采用了新的分区工具（新的Paritioner），因为我是覆盖在原来的8.02上的，所以分区的时候才用了advance的方式，所以虽然默认是ext3，不过我还是看到了ext4的选项（尽管我最后还是选择了ext3，因为ext4在windows中无法挂载）。我用ext3格式化了之前的Ubuntu8.02，挂载了"/"项。虽然用法比较暴力，不过对我来说，其他的操作系统也的确就是玩玩的吧，除了装openSUSE的时候，的确做了些项目的。（后来的结果就是不当心格掉了，囧，害的丢失了很大一部分代码）




It's just a glance. I even didn't install my video card driver. I will update the article, and if you like it, you must try it yourself. We are doing the important stuff what can save the world from Microsoft ;)




以上只是一瞥，我甚至还没有装上显卡驱动（它提示我下载的时候实验室断网了）。我会更新这篇文章的。如果你喜欢它一定要自己试一下，我们在作者从微软手中拯救这个世界的重要的事情。




在论坛上和大家交流了一下，发现其实新特性也就不过这几个，要说Xserver1.6，我倒是没觉得什么，因为我的图形还是差的啊。不知道装上了显卡驱动会不会感受到。说实话因为断网害的我显卡驱动没装，这个惊鸿一瞥也变得打了折扣。等我用了一段时间再写写心得吧。话说10月份会有9.10，到时候看来又要更新一下了。明年才是重头戏，明年会有Gnome3.0，到时候有些觉得KDE比较漂亮的人就可以好好期待一下了。呵呵。

那么，以上就是first glance了。放几张登录和partitioner的屏摄。




![](/images/uploads/zb/2009-04-24_Ubuntu_starting.jpg)






![](/images/uploads/zb/2009-04-24_Ubuntu_partitioner_1.jpg)




![](/images/uploads/zb/2009-04-24_Ubuntu_partitioner_2.jpg)
