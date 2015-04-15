---
comments: true
date: 2009-08-07 01:38:22
layout: post
slug: solve-z-blog-1-8-arwen-build-90619-br-character
title: Z-blog 1.8 Arwen Build 90619 换行符问题的解决
wordpress_id: 91
categories:
- Programing
tags:
- Z-blog
---







在7月20日后知后觉的将Z-BLOG从build 81206 升级到 build 90619 之后，就没有正儿八经的贴过代码了。以至于10多天都没有发现我的Z-BLOG已经有了细微上的变化。昨天Arthur贴一些简单的C#代码的时候，才发现pre标签里的换行都不会被记录了。百思不得其解，并且四处求助。最后求人不如求己，夜深人静的时候总是喜欢维护下博客，所以刚才解决了这个问题。详细看看我问题的发现和解决过程。




**问题发现：**




Arthur在后台是使用了Z-BLOG默认的FCKeditor来编辑文字的，应该说是非常习惯的，尤其是比较偷懒的每次使用lightbox都直接把代码贴进文章区域里，所以经常在所见即所得和源代码之间切换编辑。可是在昨天写上一篇文章《[C#随机顺序输出数字 1～9 的算法](http://arthraim.cn/post/2009/08/90.html)》的时候，突然发现，UBB中贴好的代码在保存草稿之后就大变样了。其实这个问题在之前贴《[世界编程大赛第一名的3D程序](http://arthraim.cn/post/2009/07/86.html)》文中的大规模代码的时候就已经发现，只是因为那些DEX也的确没有高亮的必要，所以后来采用了其他的方式。而昨天注定要使用高亮的时候也遇到了这个问题，Arthur就感到十分的困惑了。




进一步的发现是，我找到了以前的一些文章，比如[XNA的教程](http://arthraim.cn/post/2009/07/81.html)，这好几十篇的文章都是有很多代码的。结果情况非常可怕，所有的这些代码也都是乱乱的。看着Google Anlytics上面的访问量和搜索引擎统计我就心急，难道大家通过搜索引擎过来看见的就是这样的乱七八糟的代码（没有换行）？




之后我找到了升级博客系统前的数据库备份，找到了文章的字段的内容。将其贴出来之后发现所有的文章都是有换行信息的。于是我再重新检查当前的数据库，发现升级之前的文章的换行符也都是存在的，只是新写的文章看来是都没有了。并且老文章生成的HTML页面，那些换行符也是没有了。




最终可以把问题归结成2个。






  1. FCKeditor 存取数据库时去除了换行符。


  2. 文章重建，也就是通过UBB生成html 静态页面的时候去除了换行符。




**问题解决：**




在找到问题所在之后，Arthur开始寻找解决办法。解决第一个问题，就要看FCKeditor 的问题。找了一些资料，记得在以前找代码高亮方案的时候看到过相关的笔记，于是找出当时存的网页，看了下。果然，问题找到了。




Build 81206 的版本要插入pre，必须要注释掉去除换行符的代码。我当时找到这两行代码的时候发现其实已经注释掉了。这么说在这一版本中已经默认了保留换行符。而在Build 90619中，这部分代码又没有被注释了，不知出于什么原因又将策略改了回去。那么只要重新注释掉，就能解决FCKeditor存取的问题了。




罪魁祸首就是下面的代码。function/c_system _event.asp 文件中的第298行。这四行分别控制了文章内容(Content)和摘要(Intro)CR,LF，LF的替换。可见都被替换成了""。注释掉自己需要的就可以。我只是注释了298这一行，足矣。





    objArticle.Content=Replace(objArticle.Content,vbCrLf,"")
    objArticle.Content=Replace(objArticle.Content,vbLf,"")
    objArticle.Intro=Replace(objArticle.Intro,vbCrLf,"")
    objArticle.Intro=Replace(objArticle.Intro,vbLf,"")




的另外，联系c_system_event.asp上下文可以看到，这只是CASE "fckeditor"，如果你没有使用这个默认的编辑器，那么把其他的case也都改掉吧。




很好，解决问题1之后，重建文章还是没有效果的，因为还有问题2困扰在这里。其实Arthur并不清楚Z-blog的代码是怎么组织的，所以生成HTML的代码究竟在哪里我也不知道，毕竟我也不是开发人员只是个使用者。跑去Z-blog的官方网站的wiki，看[更新日志](http://wiki.rainbowsoft.org/doku.php?id=wiki:changelog)缩小范围。




[![](/images/uploads/zb/2009-08-07_update.jpg)](/images/uploads/zb/2009-08-07_update.jpg)




哈哈，更新不多，涉及修改的也就是function/c_system_lib.asp了。把它找出来，代码量很庞大，没关系，打开 UltraEdit 的文件比对，和之前本地没有升级的文件比较比较。轻松愉快的发现了位置所在，392行。





    Public Property Get HtmlContent
    HtmlContent=TransferHTML(UBBCode(Content,"[face][link][email][autolink][font][code][image][typeset][media][flash][key]"),"[html-japan][vbCrlf][upload]")
    End Property




恩，这里的代码也被加回来了，悄悄的把[vbCrlf]去掉。文件重建后就大功告成啦～




**小小小总结：**




面对不是自己开发的东西的版本更新带来的问题，要这样解决也是蛮辛苦的，好在WEB程序还是相对自由一点，倘若其他的桌面应用拿过来就痛苦多了（不过反汇编软件也很多很高级）。之前还求助交流论坛，还求助开发人员（虽然没有得到回复），不过好在夜深人静的时候把它解决了。




由此我联想到，笔记桌面应用来说，更加难以解决版本更新带来的问题（不能自己解决，只能等开发者下一个版本的更新）在web3.0时代就将更加常见了。上一次youtube引发的网民不买账事件我觉得应该可以载入史册了。当然也许web应用版本更新带来用户不接受的例子不是第一个，不过应该是足够有分量的一个了。未来是不是只能看着别人脸色过日子了。
