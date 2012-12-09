---
comments: true
date: 2011-04-07 20:12:47
layout: post
slug: something-about-oauth
title: 随便写写oauth
wordpress_id: 738
categories:
- Programing
tags:
- 微博
- 网易
- 腾讯
- Java
- oauth
- QQ
- 搜狐
---

最近用到了其他微博的oauth, 起初觉得oauth很烦, 因为文档看了很久很久都没有弄明白怎么回事. 等到初步模到了一些门道之后, 发现其实挺简单的, 因为那是一个标准, 理论上只要支持oauth的网站用一套代码去跑基本上都可以通过. 不过在实际的应用当中我算是领教到了中国互联网的强悍之处. 已经不记得是在什么地方看到一篇文章, 作者一上来第一句话就是 "从oauth可以看出一个互联网企业的技术力", 虽然稍微有些偏激, 但多多少少有值得认同的地方.




**我了解的oauth**




oauth其实是个认证, 就和HTTP的basic auth一样, 通过认证才能访问HTTP的资源. 之所以需要oauth, 只是为了解决一个问题:"第三方不获得用户第一方的密码的前提下访问第一方的资源", 所以保障用户通信过程中密码不被泄露是其次, 关键更重要的是第三方不会获得用户名和密码.




于是强大的地方就这样显现出来了, 不知道大家有没有看到过新浪微博的xauth, 什么是xauth呢? 我没有去考据究竟xauth是谁的创造, 它几乎搬了大部分oauth的内容, 不过解决了一个什么问题呢? 解决了一个 "oauth认证必须跳转到第一方去输入密码" 的问题(美其名曰提升用户体验). 对不起我凌乱了, 第三方是可以得到用户密码的, 那它是不是直接把oauth的第一大feature给OOXX掉了 - - 那basic auth不能满足你么大哥?




**各大网站的oauth**




因为工作和兴趣双重因素所致(这真是个难得一遇的好事情), 我接触了各个门户的oauth, 首先有个前提就是我用了oauth.net上罗列的某个java的oauth库, 饱受挫折, 经过几周的积累, 我才决定写下这篇文章, 多多少少对后来者有所提示和帮助.




**新浪微博**




把新浪放在第一家只是因为我第一个做的是新浪微博的oauth, 一上来使用了一个库叫做[oauth-signpost](http://code.google.com/p/oauth-signpost/), 看上去挺不错的样子, 尝试了一下它对Google等国外网站的例子, 我觉得可以用, 于是开始写. 之后马上就遇到了问题, 在申请request_token的过程中, 服务器会返回一个oauth_callback_confirmed的参数, 这个是明确写在oauth 1.0标准中的 (原先的1.0a, 如今已正式merge到了1.0中, 感兴趣的话[这里](http://www.skiyo.cn/2010/08/23/oauth-1-0a-and-1-0/)有一篇资料描述了1.0a和早期1.0的差异), 是必须有的, 可是如果你是通过POST方法请求, 那么很抱歉新浪微博是没有提供的. 这显然是一个疏漏. 在新浪微博官方论坛里, 这个问题不止一次的被提到(看[这里](http://forum.open.t.sina.com.cn/read.php?tid=1838), 看[这里](http://forum.open.t.sina.com.cn/read.php?tid=912&ordertype=desc),), 虽然说是 "近期会加上" 不过感觉新浪要改应该不会那么敏捷, 所以能做的只有耐心等待和回避这个问题. (虽然不影响正常功能, 不过毕竟人家是标准, signpost就是属于那种按照标准来结果在新浪出错了的牺牲品)




为了回避这个问题, 我改用了[scribe-java](https://github.com/fernandezpablo85/scribe-java)这个库. 不过在做新浪微博的时候发现的问题, 好像只有这一个.




**网易微博**




网易是在我快速尝试了搜狐/豆瓣/腾讯之后成功完成的第二个, 事实上很幸运, 基本上不用改太多的代码就完成了网易的功能. 虽然这样, 并不表示网易就按照标准来, 首先我发现的明显的一个不按照标准的地方是authorization步骤, 它提供了两个接口, 一个是/oauth/authorize, 一个是/oauth/authenticate, 首先我先申明这两个单词我一直搞不清, 不过在网易给oauth的分工中, 它们的工程师是这么定义的: authorize不处理callback, 直接展示verifier的页面; 另一个, authenticate则处理callback. (至于给第二个传oob会怎么样, 有没有返回oauth_callback_confirmed, 我都没有细看了)




另外小提一下, 它在回调你的callback地址的时候, 传的参数名是oauth_token, 不过相信我, 我觉得那是oauth_verifier.




**腾讯微博**




对腾讯微博(QQ微博)的尝试其实在网易之前, 不过才看了文档我就挂掉了, 因为它不支持header传递oauth参数, 只支持querystring, 虽然的确在oauth协议中写出可以使用querystring, 不过我觉得header比较帅气可不可以啊, 而且我的库不支持querystring啊企鹅大哥 - -




大问题放前面, 后面还有小问题呢. 在请求request_token时传递oauth_callback参数, 因为写测试没用到callback, 所以肯定库会帮我填上oob(out-of-bound), 不过让我很崩溃的是企鹅大爷不高兴了, 给我跳转到了一个这样一个地方: https://open.t.qq.com/oauth_html/oob?oauth_token=xxxxx&oauth_verifier=xxxxxx, 这当然是NOT FOUND, 这是企鹅大哥的地盘, 我不能做主啊.

千辛万苦查到了企鹅大哥的文档里写了这么一句话: __① 用户授权后web应用将会重定向到oauth_callback。当应用为pc客户端或手机客户端应用时，没有回调url(oauth_callback)的概念，此时设置为字符串null即可。字符串"null"必须是小写。__爹啊... 你是我亲爹啊. 不按套路出牌的啊! ([愤怒的同学](http://open.t.qq.com/bbs/viewthread.php?tid=2352)显然不止我一个, 而且没人理他)




对了, 今天还发现腾讯微博开发平台的文档修改过了, 不过.............. 被修改的似乎只有文档而已.




**搜狐微博**




申请搜狐博客应用的时候填写了一个callback, 不知道是干吗的. 另外看了一些人写的东西, 说搜狐不让传realm. 我用之前的代码跑了一下, 结果报错说signature_invalid, 我想这东西估计不靠谱就先放下去做网易微博了. 今天下午回过头来去再摸了摸搜狐微博, 想不到一下子就跑通了, 可喜可贺, 不知道是不是它最近几天有更新, 还是我第一次的尝试实在太潦草以至于自己出错了错怪了搜狐. 不过搜狐微博的开发文档写的真不怎么样, 相比起来前三家好多了.




还有一个问题是搜狐不理会我的oauth_callback, 除非在authorize的时候传(querystring, 和oauth_token一起传), 不过这应该是oauth 1.0a就改掉的地方, 搜狐大概是没有跟上?




**豆瓣**




你们知道, 作为一个程序员, 对豆瓣的崇拜之情是难以言表的. 豆瓣的高性能, 以及豆瓣为开源做出的各种贡献都是围城外面的程序员羡慕不已的, 并且把 "指环王" 当作文化的豆瓣团队也给我留下了足够美好的印象. 熟悉python的朋友不会不知道simple is better than complex这句话, 用python开发的豆瓣无论如何都让我觉得是simple的典范. 无论如何, 我始终觉得豆瓣是一个技术很棒的企业. 这一切美好的印象只持续到我接触它的oauth之前.




还是先说有点, 豆瓣的oauth文档简介明了, 该说的都说清楚了. 另外豆瓣的错误信息返回的很厚道, 会把服务器拼出来的base string发给你, 让你和自己的好好比较一下, signature错误找起来就方便多了.




不过signature的错误你是永远也不会觉得好办的... 因为豆瓣太特别了. 在它的basestring里有一个多余的 "OAuth%2520" 请求的url之后, 所有参数之前. 百思不得其解这究竟是什么, 难道是历史原因造成的? 总之对于我来说要做的只不过是拼接basestring的时候硬加上去. (至今仍然希望高手解答, 之前的oauth版本里是这样的?)




解决了这个问题还没完, 还有问题. 我尝试使用拿回来的access_token去访问资源, 又报signature错误了. 具体问题我在豆瓣的[这个帖子](http://www.douban.com/group/topic/18667209/)里详细描述了, 不过还没有得到答案. 在这个请求中我要不要加上oauth_callback_url这个参数是一回事, 它是不是参加排序又是另外一回事不是么? (至少不该是签名错误吧)




另外还有一个小问题, 豆瓣返回oauth_token=xxx&oauth_token_secret=xxx也是排序错误的, 两个位置对调了. scribe-java是用正则去匹配的, 所以出错了.




总之豆瓣折腾的最疼, 难免心里有些失望.




**总结一下**




oauth的应用越来越广泛, 互联网企业拿着互联网开发的新年开发着自己的平台, 这是一件很好的事情.




从开发角度来说, 不得不承认oauth并不简单, 我也并不精通oauth, 偷懒了用了别人的库或多或少出现了些问题. 我依然比较郁闷的是, scribe-java本身自己就支持一大堆国外的oauth, 并且在我看来代码都是没有什么问题的, 到了国内, 标准就不是标准了, 标准只是参考, 各家各户按照自己的理解在做. 这篇文章写完, 我也有些心虚, 一些问题可能还是我有错, 如果你发现了, 欢迎拍砖我赔礼道歉.




最后提一下, 在[github上](https://github.com/Arthraim/scribe-java)把scribe-java给folk了, 添加了4大门户的微博, 欢迎其他开发者使用或贡献.



