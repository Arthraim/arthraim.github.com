---
comments: true
date: 2009-09-17 02:17:53
layout: post
slug: official-renjian-widget-javascript
title: 官方的人间网Widget - Javascript
wordpress_id: 103
categories:
- internet
tags:
- 人间网
- javascript
- widget
---







说到前文《[简单的人间网Widget - javascript](http://arthraim.cn/post/2009/09/100.html)》，之后想写一个像样点的widget的想法一直在Arthur的脑子里转啊转，不过就像之前提到的那样，去学校了一段时间，搞定一些烦人的事情，所以搁下了。今天看到了某人间网工作人员的留言，方知人间网已经有了官方的widget，拿来炫耀炫耀，话说有什么值得我来炫耀的吗？嘿嘿。




看看效果先。





不错的吧，样式是和人间网的整体风格相统一的，说实话对于有官方情结的人来说这主题还是很有爱的。




```html
<script src="http://rensea.com/js/renseaApi.js?screenName=arthraim&width=550" type="text/javascript"></script>
<script src="http://rensea.com/statuses/user_timeline/arthraim.json?count=10&callback=renseaApiCallback" type="text/javascript"></script>
```




只是在自己博客用之前，还要改造一番，兴许等我改了之后又能再发一篇分享一下。




说到用法，把两行代码放到你要放widget的地方。代码要稍稍修改一下：




```html
<script src="http://rensea.com/js/renseaApi.js?screenName=你的名字&width=你要的宽度" type="text/javascript"></script>
<script src="http://rensea.com/statuses/user_timeline/你的名字.json?count=你要的数量&callback=renseaApiCallback" type="text/javascript"></script>
```





此文还真是很水…… 不过我现在可以写一篇非常长的心得来告诉大家不更新对于访问量和SEO的坏处，所以，还是允许我水一下吧。




PS：话说Chrome还蛮有意思的，renseaApi.js里的一些样式和我的页面样式有冲突，比如链接的颜色，于是可以看到有趣的样子。Firefox 倒是很正常～




以上……
