---
comments: true
date: 2012-02-12 20:27:59
layout: post
slug: mac-os-10-7-3-breaks-xcode-binary-validation
title: Mac OS 10.7.3用xcode提交二进制文件验证失败
wordpress_id: 775
categories:
- Programing
tags:
- appstore
- xcode4
---

近日在提交一个app的时候发生了奇怪的错误。错误如下：




[![](/images/uploads/wp/icon-dimensions-dont-meet-the-size-requirements.png)](/mac-os-10-7-3-breaks-xcode-binary-validation/icon-dimensions-dont-meet-the-size-requirements/)




>

>
> iPhone/iPod Touch: Icon.png: icon dimensions (0 x 0) don't meet the size requirements. The icon file must be 57x57 pixels, in .png format
>
>





我使用的Xcode版本是4.2.1，其实一下子也懵了完全没头绪。我第一时间当然是以为我的Icon.png真的出问题了，因为偶尔我会自己用一个叫做prepo的app来把512的图标转成57像素的，所以我第一时间以为是它的问题。（因为它之前也把比如10k的blabla@2x.png转成了20k的blabla.png）。于是我用Photoshop重新转换了一个图标来试图解决这个错误，结果当然是失败了。




各种Google之后，问题被锁定到了Mac OS 10.7.3这个升级包上，升级了之后是第一次提交二进制文件。[这个讨论](https://devforums.apple.com/message/611733)都已经说明了病灶在哪里。




Xcode提交当然还是存在问题，无法通过验证，而且暂时没法解决。但是办法还是有，使用更加原始的Application Loader来上传。于是我打开Application Loader并登陆之后问题又出现了。




[![](/images/uploads/wp/Screen-Shot-2012-02-10-at-12.16.13-PM.png)](/mac-os-10-7-3-breaks-xcode-binary-validation/screen-shot-2012-02-10-at-12-16-13-pm/)




嗯，自带的版本看来太老了。于是各种Google之后[新的版本](https://itunesconnect.apple.com/apploader/ApplicationLoader_2.5.1.dmg)终于还是找到了。使用方法很简单，跟着向导一步一步做（前提是itunes connect上的应用已经waiting for upload状态）直到需要在硬盘选择文件。首先在Xcode里Product -> Build for -> Building for archiving。然后Xcode工程下Products目录，有个 工程名.app，右键Show in finder，在它的上一级目录里找到release版本的app。当然上传前要把它打包成zip文件，application loader不能指定.app。最后完成向导，文件终于上传成功鸟 -。- 然后可以等着review了。




**更新：**好吧看到了新说法是只要装好Application Loader 2.5.1，重启xcode，clean工程，重新archive，然后就能通过验证了，不需要真的使用Application Loader上传。 没试过，不一定管用，可以试试看。




以上。



