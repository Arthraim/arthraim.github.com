---
comments: true
date: 2009-09-04 19:17:08
layout: post
slug: simple-widget-javascript
title: 简单的人间网Widget - javascript
wordpress_id: 100
categories:
- Programing
tags:
- 人间网
- javascript
- widget
---




twitter官方提供有一个widget，只要稍加修改就可以结合到自己的网站中。但是很不幸，twitter被墙之后，即便是不用域名直接用ip也没办法继续使用了。网上有各种各样的朋友使用各种各样的方法逾越这道墙，不过我已经提不起精神了，一个IP没用了就会有两个，也许我们挖空心思完成的方法，对于他们只是在列表里多加一个几个IP而已（当然twitter还是一直在用的啦，只有API还在，办法总是有滴）。所以我改用人间网来展示我的tweets。人间网的一大优点就是有一个"足迹"，可以同步其他服务的feed，只是滞后性是其诟病。




人间网是个不错的微博，据说焦点集中在对微博中IM特性的提升，欢迎各位看官们在人间网上关注我@[Arthraim](http://rensea.com/arthraim)。这次因为人间网API支持了callback 参数，所以我把twitter的widget 精简了一下，写了下面这个widget，callback只是在返回的json外面套了一个函数方法的"套子"，不过这就让他在javascript 的表现上更加自由方便了。以下是效果展示：




* * *




![](/images/uploads/zb/rensea_logo.png)





<ul id="rensea_update_list"></ul>


* * *




因为人间网的API 和twitter 很相像，所以其实不用做什么大修改，稍微改改就可以了。@用户和URL的正则都是不用变的，URL有没有修改我也忘了，当然域名是肯定要改掉的。另外生成HTML的时候我把它最大化的简化了，只留下了status的内容（人间网对链接图片的处理有所不同）。以下代码。



```js
function renseaCallback(statuss) {
    var statusHTML = [];
    for (var i=0; i<statuss.length; i++){
        var username = statuss[i].user.screen_name;
        var status = statuss[i].text.replace(/((https?|s?ftp|ssh)://[^"s<>]*[^.,;'">:s<>)]!])/g, function(url) {
        return '<a href="'+url+'">'+url+'</a>';
            }).replace(/B@([_a-z0-9]+)/ig, function(reply) {
                return  reply.charAt(0)+'<a href="http://rensea.com/'+reply.substring(1)+'">'+reply.substring(1)+'</a>';
            });
        statusHTML.push('<li>'+status+'</li>');
    }
    document.getElementById('rensea_update_list').innerHTML = statusHTML.join('');
}
```



至于HTML只要一个id为"rensea_update_list"的ul就可以了。把上面的js代码复制下来存为rensea.js（或者你直接用我的 http://arthraim.cn/THEMES/techified/SCRIPT/rensea.js），另外的URL请参照[人间网API](http://rensea.com/api.html)修改自己需要的。



```html
<ul id="rensea_update_list"></ul>
<script type="text/javascript" src="rensea.js"></script>
<script type="text/javascript" src="http://rensea.com/statuses/user_timeline/arthraim.json?callback=renseaCallback&count=20"></script>
```



那么大概就是这样。如果要加上原来显示的时间的话，json中的时间格式是类似于这样的：2009-09-04 01:41:49。parse前要把2009-09-04改成09-04-2009，我觉得时间也没必要，就简单一点吧～ 愿意的话返回的信息都能用上。




以上。
