---
comments: true
date: 2011-06-29 03:10:44
layout: post
slug: the-update-of-rubygems-causes-yaml-parsing-problem
title: RubyGems升级造成yaml parse错误
wordpress_id: 755
categories:
- Programing
tags:
- mongodb
- mongoid
- rails
- ruby
- yaml
---

遇到个很蛋疼的问题，mongoid的配置文件（一个YAML）文件引发了下面这个错误：




> 
	
> 
> db_name must be a string or symbol  

		...MONGOID_PATH/lib/mongo/util/support.rb:50:in `validate_db_name'
> 
> 





Google到[这里](http://cstrahan.com/2011/05/14/mongoid-yaml-rails-conflict.html)，知道原因是RubyGem v1.5.0开始把默认的yaml parser从syck改成了psych。




解决方法一当然是改你的yaml让它可以被parse，如果需要使用之前的parser，那要在config/environment.rb中加上以下两行




> 
	
> 
> require 'yaml'  

		YAML::ENGINE.yamler= 'syck'
> 
> 





话说这好像是第一次写ruby/rails的文章，刚刚玩了两三个星期~




以上



