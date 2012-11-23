---
comments: true
date: 2010-03-13 00:12:57
layout: post
slug: renjian-api-apps-summary
title: Renjian API应用小汇总
wordpress_id: 144
categories:
- internet
tags:
- API
- App Engine
- 人间网
- Python
- renjian
---

在[人间网](http://renjian.com/dd/reg?verify_code=NQhk78ByzDzLZzVroXfp)实习，很开心。利用人间网的API，我可以自己磨叽磨叽写一些我想写的东西，其实是很快乐的（并且罪恶的没有压力的）。在人间网混了有段时间了，也认识了很多朋友做了很多的各式各样的应用，这里我就来小汇总一下，有漏掉的我没有察觉的注意的，请同学们联系我~







**@**[**Arthraim**](http://renjian.com/arthraim)：不用说，先要介绍的是来自本人的作品，我也写了一些个小东西，让我当一回王婆吧（我本来就这个姓 = =）






  * [人间浮云（renjian-deck）](http://deck.renjian.me)：这是人间现在唯一的功能比较全面的客户端了。是用AIR/ajax（jQuery）实现的，是我用人间写得第一个应用，基于开源twitter客户端ada。所以它的代码也是在[这里](http://github.com/Arthraim/renjian-deck)上开源的~


  * [人间邻居（renjian-neighbour）](http://neighbour.renjian.me)：通过你的screen_name，找到你在人间网上的邻居。是个实现异常简单，但是还蛮有意思的应用，是我学习django框架过程中的小实践。源代码可以在[这里](http://github.com/Arthraim/renjian-neighbour)找到。


  * [人间头像（renjian-avatar）](http://avatar.renjian.me)：这个和人间邻居类似，也是django学习中写的，也是利用API没有的一些小hack实现的有意思的功能。是查看你所有在人间使用过的头像~ 自己颇为喜欢的一个应用，同样实现简单，不过很有乐趣。同样也是[开源](http://github.com/Arthraim/renjian-avatar)的。


  * [renjian-python](http://github.com/Arthraim/renjian-python)：这是renjian API的一个python wrapper。是我偷懒用了python-twitter的代码改了改完成的，现在github有个分支叫做nocache是剥掉了缓存逻辑的版本，这个版本我自己正在做的一个东西也在吃狗粮，所以还在不断的更新当中，欢迎大家贡献~ 代码在[这里](http://github.com/Arthraim/renjian-python)。







**@**[**jlinux**](http://renjian.com/jlinux)：jlinux写了一个[android平台的人间客户端](http://renjian.com/tools/android)，以及一个完成一半的iPhone版本。可以直接发言，回应等，也可以马上拍照上传，等等。是android手机用户的强大客户端，也是人间网非常早期的一个API应用。不过随着人间网的更新发展，这个客户端的一些功能已经滞后于人间网的发展了，期待作者的进一步更新。  android的代码也开源了~ 源代码可以在[这里](https://code.google.com/p/renjian/)下到。







**@**[**F0ur**](http://renjian.com/f0ur)：小四同学也是人间网非常非常热心的好朋友~ 也是对编程充满热情的同学，为人间的api应用做出了很多很多的贡献，于是我也来列举一下吧。






  * [人间饭票](http://renjian.f0ur.name/ticket/)：人间饭票是一个非常非常可爱的应用，可以把最近自己的发言生成一张图片，方便贴到自己的blog或者是论坛签名上，小小的一张很像饭票，很可爱的名字。很遗憾的是，因为人间饭票正在经历很大的版本更新，所以现在暂时没有在服务，希望小四同学尽快放出新版吧。顺便一提，人间饭票的视觉设计师@[粢饭](http://renjian.com/%E7%B2%A2%E9%A5%AD)同学做的，非常非常的漂亮！[这个](http://renjian.f0ur.name/ticket/)链接是将来会使用新版本的地址，期待小四的更新吧。


  * 天气机器人@[weather](http://renjian.com/weather)：这是小四开发的人间网上的一个用户，是一个机器人，只要你说"XX 天气"，它就为告诉你它支持的地域的天气预报等等信息。同样小四使用python实现的，代码也同样在[这里](http://github.com/F0ur/renjianWeatherRobot)可以找到~ 人间开发者都好爱开源啊，感动~


  * [Tw2other-GAE-python](http://github.com/F0ur/Tw2other-GAE-python)：Tw2other是@cuies同学开发的一个micro-blogging同步工具，小四同学为大家做了一个GAE版本的可以轻松部署到GAE上，每个人都能实现自己的专属同步工具。源代码可以在[这里](http://github.com/F0ur/Tw2other-GAE-python)找到。另外，有个简单的介绍，注册了人间网的同学们可以看[这个对话](http://renjian.com/c/768615?page=1)。







**@**[**format**](http://renjian.com/format)：硬盘被格同学的blog挂了，这位同学开发的一个Chrome插件客户端也是很早就开发出来的人间API周边，可以在Chrome监视人间。[这里](https://chrome.google.com/extensions/detail/hehijbfgiekmjfkfjpbkbammjbdenadd)可以下载，源代码有没有开放不太清楚，不过也是基于一个开源项目。这个应用我用过，功能上可能和人间还有一定的出入~ 还有很大的修改空间。







**@**[**asfman**](http://renjian.com/asfman)：说起Chrome插件，有一个非常优秀的作品，来自人间团队成员。人间有个Bookmarklet，可以分享站外内容到人间，非常好用方便。asfman为我们写了一个Chrome插件版本，不喜欢特地留着bookmark那一栏的同学，也可以轻松使用这个功能了，漂亮的按钮，让这个应用更加实用。[这里](https://chrome.google.com/extensions/detail/jlniemlgnlgpcejmnecphegkcalebkhf)可以下载到。







**@**[**redjackwong**](http://renjian.com/redjackwong)：这位同学最早在人间叫"贾君鹏"，现在是人间起哄的，绝对是第一起哄的，他为人间开发过一个基于.net 3.5的应用 ---- [人间风景](http://www.cnblogs.com/redjackwong/archive/2010/02/02/RIV_20100202_GUI_Preview.html)（RIV, Renjian Image Viewer）。值得一提的是最近Google Reader新推出的Play，和起哄同学的RIV的体验非常的相似，这位同学看来走在Google前面了~







**@**[**jones**](http://renjian.com/jones)：囧死同学是最近在人间冒出来的一个好同学，目前为止为人间开发了两个有趣的应用。






  * [renjian.info/app](http://renjian.info/app)：这是一个查看好友情况的应用，比如你关注的好友几个关注了你，几个没有关注你等等各种相关信息。bonus，还有一个看人间网当前有多少用户的小功能，受到了一些有窥探机密欲望的同学的青睐。


  * renjian.info/app/follow.php：这是一个不能给链接的应用，因为关键就是一个连接，让你一键关注某个用户。比如我在我的博客上可以放一个，[http://renjian.info/app/follow.php?id=arthraim](http://renjian.info/app/follow.php?id=arthraim) 这样的链接，点一下然后登陆，于是就马上能跟随我了，对人间网用户之间的交流很有好处。







相信还有很多同学想，或者已经在开发一些有趣的人间API的应用，和twitter API类似，人间的API让开发者有了非常棒的功能，可以实现很多想象力之外的新奇玩具。我不是在给人间做广告，只是作为一个人间网玩家，我很愿意做这样一个汇总~




以上~
