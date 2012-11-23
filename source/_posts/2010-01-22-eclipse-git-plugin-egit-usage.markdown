---
comments: true
date: 2010-01-22 16:11:43
layout: post
slug: eclipse-git-plugin-egit-usage
title: eclipse的git插件Egit使用小记
wordpress_id: 136
categories:
- Programing
tags:
- eclipse
- git
- github
- SSH
---

从WindowsXP切换到Ubuntu910，一切又陌生又熟悉，装过无数Linux的版本，可是真正深入的用过几个倒是没有，大多数都是玩一下，除了玩mono的时候好好的体验了一把openSUSE。话说这次把所有的工作都切换到Linux上来，的确是遇到了很多问题，不过拿出对付Linux的精神，处理什么问题的能力都是会得到提高的。这次也遇到了一些问题，不过还是坚持想用eclipse里的git插件来完成所有的github的工作，你依赖图形吗？常年用Linux的我看都不怎么依赖…………




依照[这个Guide](http://github.com/guides/using-the-egit-eclipse-plugin-with-github)可以完成所有的工作，包括2009年10月更新了[一个issue](http://code.google.com/p/egit/issues/detail?id=104)的solution。具体就是遇到了"Cannot list the available branches. Reason: git+ssh://git@[host]:[...]/[...].git: Auth fail" 这样一个错误。解决方法就是把$HOME/.ssh在原路径下复制一个，改名为ssh就好了。Windows里也是一样，把C:Documents and Settings{user}.ssh这个文件夹复制一个C:Documents and Settings{user}ssh。这样就直接可以图形上push了，非常的方便。




值得一提的是，因为要把之前windows上的东东弄到linux里，所以在import git repo的时候，我神奇的发现新的工程是不存在的，项目工程还要再导入一次，像我这样没有同步.project等文件，pydev又不能从现有文件导入成到新工程，就只能重新自己拼出这些文件了，好在写起来比较简单，建个新的工程改改名字就好了（和可恶的配置java工程比起来好像简单多了啊，3个多月了，至今我对Java依然望而生畏）。




记上一笔～ 本来想多写点，不过那两个链接里啥都有了…………




以上……



