---
comments: true
date: 2010-11-03 22:39:36
layout: post
slug: disable-ckeditor-html-entities-in-the-output
title: CKEditor的转义什么的最讨厌了！
wordpress_id: 632
categories:
- Programing
tags:
- CKEditor
- 语法高亮
- 转义
- Wordpress
---

话说程序员的博客总是用到SyntaxHighlighter之类的来在pre标签里贴一堆代码。于是因为装了CKEditor for Wordpress就一直觉得很讨厌，在HTML标签里写下一些代码，到了Visual里就被转义了。比如>和<就变成了&gt;&lt;虾米虾米的。话说今天心血来潮去Google了一下，CKEditor的设置文档里还真的有相关的设置，请围观[这里](http://docs.cksource.com/ckeditor_api/symbols/CKEDITOR.config.html#.entities)！




配置ckeditor插件目录下的ckeditor.config.js文件，加入下面这行。



    
    config.entities = false;




不过这样比较暴力，所有的内容都不转义了，有没人有担心。于是在[这里](http://chiefleo.me/archives/306)查到还有另外一种办法（如下），用正则筛选要保护的代码。这样对数据来说是很不错，不过有个缺点，在ckeditor所见即所得模式里看不见被保护的内容，这个就看你自己的取舍了。



    
    config.protectedSource.push(/<pre[sS]*?pre>/g);




貌似我还是倾向于用前者~ 在所见即所得标签里黏贴的内容还是都会转义的，只是HTML过来的内容都不转义了，貌似不错。独立博客真是要折腾啊 = =  怪辛苦的……




以上。
