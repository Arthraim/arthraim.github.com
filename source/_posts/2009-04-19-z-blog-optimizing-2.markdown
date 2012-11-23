---
comments: true
date: 2009-04-19 05:35:28
layout: post
slug: z-blog-optimizing-2
title: Z-blog的美化——之二
wordpress_id: 12
categories:
- Programing
tags:
- Z-blog
---




周末通宵，有益身体健康。囧。看了一集非常精彩的House M.D.之后，一时间觉得没有什么可做的，于是来到博客，东看看西看看想给他做个美化。无意间去了iNove这个主题的作者mg12的[这个](http://www.neoease.com/)主页，觉得很棒，是iNove的进化版了吧。然后看到他发布iNove的页面，顿时发现我老土了，原来原先的iNove这么漂亮的，被移植到Z-Blog的时候看来是被阉割了，[这里](http://demo.neoease.com/)可以看到原来的样子（就是demo页）。不去说他填充的内容好了，有几个地方明显比我的好看。左下角的WordPress的图标，RSS订阅的小面板，还有About下面的下拉菜单。都是用Javascript实现的小功能，觉得很不错。那天就很困惑iNove文件夹里面的script目录中的三个js文件究竟有没有用进去，今天一看，原来都没有，文件摆在那里呢就～ 好！那么动手一个一个的加回来 ---- iNove补完计划正式开始。（ 之前做了一些美化请看[这里](http://arthraim.cn/post/2009/04/5.html)）




补完计划 第一项 ---- 左下角加上个自己喜欢的LOGO。




首先做的就是分析原来Demo和我的这里的区别。用Chrome的右键Inspect Element很容易定位他的代码（个人习惯）。




![](/upload/2009-04-19_Powered_1.png)




原来很简单，就是在copyright的div上面加一个标签a而已，id是powered，那么文章就是在CSS里了咯～ 找到他的CSS文件（提供下载的不是偷下来的 orz），发现了a#powered标签的代码，拷贝过来的时候一看，额，我们的inove.css里面原封不动的写着呢，好吧移植一般的确不会去改CSS文件吧。那么好，CSS的基础有了，只要在TEMPLATE上加上去就好了。后面的工作就简单了。还是把代码贴一下，终于可以插入代码了，呵呵。






    
    a#powered {
    background:url(img/Zblog-logo.png) no-repeat;
    display:block;
    width:92px;
    height:57px;
    float:left;
    margin:0 10px 0 5px;
    text-indent:-999em;
    }
    




把文件名改了，长度和宽度不改了，你要是愿意，可以怎么变态怎么改，看看会错位到什么程度比较美，我就保守主义吧，囧。HTML里面也要加入一些内容，其实后台里面也可以加，不过一般人都愿意去改TEMPLATE吧～（要不怎么叫定制呢）拷贝他的代码过来，改动一下，链接Z-Blog的主页，也对得起Powered这个标签名了。想在TEMPLATE里面贴的时候发现最初移植过来只是把这个代码注释掉了，把注释去掉，路径改下，OK，大功告成了。代码还是贴一下吧。



    
    <a id="powered" href="http://www.rainbowsoft.org/">Z-Blog</a>
    




改了下路径而已～ 做到这里，我们补完计划的第一项就实施完毕了，如你们现在看到的一样，有了这个图标。![](/upload/2009-04-19_Powered.png)










补完计划 第二项 ---- RSS订阅的小面板




觉得从feedsky上弄过来的很难看，而且堆在sidebar也的确不见得方便，何况我挺喜欢原来的图标。跑去demo一看才知道，原来这里是有面板的，这不就方便多了么。说干就干，一看就知道肯定是js文件在背后捣鬼，找一下，就在menu.js里面，好的，那么引用进来。



    
    <script language="javascript" src="<#ZC_BLOG_HOST#>themes/<#ZC_BLOG_THEME#>/SCRIPT/menu.js">
    </script>
    




之后就是面板的写法了，看看那里怎么写的，无非是多写一个div而已，控制交给js我们不需要做什么，地址换成自己的，id啊class啊和CSS和JS里对上号就行了。此div的写法如下。






    
    <div id="subscribe">
    <a id="feedrss" title="Subscribe to this blog..." href="http://feed.arthraim.cn/"><abbr title="Really Simple Syndication">RSS</abbr> feed</a>
    <ul id="feed_readers">
    <li id="google_reader"><a class="reader" title="Subscribe with Google" href="http://fusion.google.com/add?feedurl=http://feed.arthraim.cn/"><span>Google</span></a></li>
    <li id="youdao_reader"><a class="reader" title="Subscribe with Youdao" href="http://reader.youdao.com/#url=http://feed.arthraim.cn/"><span>Youdao</span></a></li>
    <li id="xianguo_reader"><a class="reader" title="Subscribe with Xian Guo" href="http://www.xianguo.com/subscribe.php?url=http://feed.arthraim.cn/"><span>Xian Guo</span></a></li>
    <li id="zhuaxia_reader"><a class="reader" title="Subscribe with Zhua Xia" href="http://www.zhuaxia.com/add_channel.php?url=http://feed.arthraim.cn/"><span>Zhua Xia</span></a></li>
    <li id="yahoo_reader"><a class="reader" title="Subscribe with My Yahoo!"    href="http://add.my.yahoo.com/rss?url=http://feed.arthraim.cn/"><span>My Yahoo!</span></a></li>
    <li id="newsgator_reader"><a class="reader" title="Subscribe with newsgator"    href="http://www.newsgator.com/ngs/subscriber/subfext.aspx?url=http://feed.arthraim.cn/"><span>newsgator</span></a></li>
    <li id="bloglines_reader"><a class="reader" title="Subscribe with Bloglines"    href="http://www.bloglines.com/sub/http://feed.arthraim.cn/"><span>Bloglines</span></a></li>
    <li id="inezha_reader"><a class="reader" title="Subscribe with iNezha"    href="http://inezha.com/add?url=http://feed.arthraim.cn/"><span>iNezha</span></a></li>
    </ul>
    </div>
    




好了然后就是图片，在我们这里也有，css的链接也是对的，这样全部做好之后就应该像我一样可以用了。![](/upload/2009-04-19_RSSpanel.png)










补完计划 第三项 ---- 下拉式菜单




刚好我装了两个相册，picaza和attachgallery，一个是看我的picasa相册（之前《[Z-blog的美化](http://arthraim.cn/post/2009/04/5.html)》里写到过），还有一个是只看上传的图片的相册，放在那里一堆刚好有些重复，不如就用下拉菜单把他们归到picture下面吧，看看怎么来做。  

	 先要找的还是js代码，这次找不到真是笑掉大牙了，menu.js在刚才就用过了，里面附属的功能都用到了menu这个命名的功能怎么会找不到呢。所以文件就不用在意什么了，我们已经做好了。  

	 需要做的是插入一段代码，把它变成下拉菜单啊。仔细一看原来的代码，恩又是很简单的，<li>里面套一层<ul>然后再在里面堆放<li>就好了，看看menu要那里改呢？恩，后台就可以，直接在后台写代码吧～



    
    <li><a href="<%=ZC_BLOG_HOST%>">Index</a></li>
    <li><a href="#">Picture</a>
    <ul>
    <li><a href="<%=ZC_BLOG_HOST%>picasa.html">Picasa</a></li>
    <li><a href="<%=ZC_BLOG_HOST%>Gallery/">Gallery</a></li>
    </ul>
    </li>
    <li><a href="<%=ZC_BLOG_HOST%>guestbook.asp">GuestBook</a></li>
    <li><a href="<%=ZC_BLOG_HOST%>tags.asp">TagCloud</a></li>
    <li><a href="<%=ZC_BLOG_HOST%>search.asp">Search</a></li>
    <li><a href="<%=ZC_BLOG_HOST%>cmd.asp?act=login">Admin</a></li>
    




预览了一下着色好像不太对，不知道发表了怎么样。对了，话说加好这个代码就大功告成了，我们第三个项目也做完了。![](/upload/2009-04-19_MenuList.png)







到这里，就把那里看得眼红红的功能全都实现了，说起来我们其实自己一行代码都不用谢，copy一下就好了，只是我摸索的时候倒是一行代码一行代码的读下去，现在这里讲起来觉得还真是简单，其实一行都不用谢，就算是拷贝也不用改TEMPLATE，在版权那里都加上就没有什么问题的说。那么第二次比较大的美化工程就到这里结束了说～ 用户体验更好了呢～




发现除了menu.js里的功能外，其他所有的JS都没用了，包括着色和现实访问数的代码。有待解决……




**更新：**红字所描述的问题已经解决。其实很简单，就是在页面末端的JS代码append之类的工作做之前我们的windows.onload已经执行了。解决方法也很简单，把menu.js放到文档最末，这样对于载入速度应该也会有一定的提高。
