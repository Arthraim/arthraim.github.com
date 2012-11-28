---
comments: true
date: 2009-09-18 21:22:44
layout: post
slug: more-renjian-widget-javascript
title: 继续人间网Widget - Javascript
wordpress_id: 104
categories:
- Programing
tags:
- 人间网
- javascript
- widget
---




又要说到前文《[简单的人间网Widget](http://arthraim.cn/post/2009/09/100.html)》和《[官方的人间网Widget](http://arthraim.cn/post/2009/09/103.html)》，第一篇文章我是拿出javascript的三脚猫功夫把twitter widget 改成了人间网的widget，第二篇文章是看到了第一篇的一个回复，人间网官方的一个widget，当时说道："只是在自己博客用之前，还要改造一番，兴许等我改了之后又能再发一篇分享一下。" 没错，这一次就是来做什么事情的。




因为官方提供的widget不但包括了核心功能还提供了各种样式，保留了人间网的自己的风格。但是Arthur是把各种各样的widget都要融入自己的博客主题中的，所以就悄悄的向renseaApi.js这个文件开刀了。我尽可能的保留了twitter widget的风格，只让javascript更新一个ul的内容，而不提供任何的div及样式，使得如何使用这个ul取决于我。




注意看现在我已经在使用的右侧的RENSEA这个模块。




[![](/images/uploads/zb/2009-09-18_rensea_widget.jpg)](/images/uploads/zb/2009-09-18_rensea_widget.jpg)




那代码的话，我就贴出所有的我的正在调用的这个rensea2.js。就像之前说的，Arthur保留了twitter只填充ul里li的这一方式，使得结合主题更加灵活。另外，所有的更新的现实方式没有对renseaApi.js作修改（除了"通过网站"）。




    function renseaCallback2(statuss) {
        var sData = [], sHtml = "";
        for (var i=0; i < statuss.length; i++){
            sHtml = '<li>';
            sHtml += '<div class="text">';
            if(statuss[i].text){
                sHtml += statuss[i].text.replace(/@(.+?)(?=s)/g, "@<a href='http://rensea.com/$1' target='_blank'>$1</a>");
            }
            if(statuss[i].status_type == "LINK"){
                sHtml += "<a href='" + statuss[i].link_url + "' target='_blank'>" + (statuss[i].link_title||statuss[i].link_desc) + "</a>";
            }
            if(statuss[i].status_type == "PICTURE"){
                sHtml += "<a href='" + statuss[i].original_url + "' target='_blank'>图片</a>";
            }
            sHtml += '</div>';
            sHtml += '<div class="timeAndWay">';
            sHtml += statuss[i].relative_date + " 通过" + (statuss[i].source=="网站"?"人间网":statuss[i].source);
            sHtml += '</div>';
            sHtml += '</li>';
            sData.push(sHtml);
        }
        document.getElementById('microblog_list').innerHTML = sData.join("");
    }




HTML调用相对简单一点，不需要有什么特别的参数，宽度就让CSS去处理吧。callback参数的方法renseaCallback2当然就是js里面的函数名，不过2是为了防止和之前简单的那个冲突。




    <script type="text/javascript" src="rensea2.js"></script>
    <script type="text/javascript" src="http://rensea.com/statuses/user_timeline/arthraim.json?count=10&callback=renseaCallback2"></script>





CSS就不用贴了吧。




以上……
