---
comments: true
date: 2011-07-20 21:11:17
layout: post
slug: nomethoderror-undefined-method-specifications
title: 'NoMethodError: undefined method `specifications'' ...'
wordpress_id: 759
categories:
- Programing
tags:
- exception
- gem
- rails
- rake
- ruby
---

今天在和同事说，我最喜欢写这样的文章了，很多人会碰到的异常，而且没有什么中文资料，可以给搜索引擎个好排名（喂你心机好重。好吧其实我的想法很伟大的哦，我是给以后会遇到一样问题的你造福哦~

重新搭新环境，先装了rvm，然后装上gem，用gem装了rails。之后呢问题就出来，尝试用bundle exec rake db:seeds的时候会出错，加上--track参数一看大概是这样子。

```
undefined method `specifications' for "/Users/arthraim/.rvm/gems/ruby-1.9.2-p290":String
```

好像后面也不是特别重要了，只是这个问题比较费解，之前同事好像也遇到过。去Google了一下，居然是gem版本太高的关系，要把gem降级到早一些的版本。不想深究下去了，就说一下解决的办法好了。

```
gem update --system 1.7.2
```

之前我的版本是1.8.5，虽然不知道具体什么情况，不过1.7.2就可以了。话说gem update到老版本的方法好神奇 -。-
