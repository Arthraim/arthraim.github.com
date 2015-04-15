---
comments: true
date: 2011-09-11 12:40:56
layout: post
slug: ruby-is-horribl
title: ruby好可怕
wordpress_id: 762
categories:
- Programing
tags:
- rails
- ruby
---

接触ruby时间不长，用的机会就更少了，所以偶尔用一下的时候难免还要去翻翻文档什么的。维护某个后台的时候，有个随机输出的需求，所以我写了下面这样一段代码。



```ruby
    def index
      ver = params[:version] == nil ? 'i' : params[:version]

      @preview_items = MyItem.where({:version => ver, :order.gt => 0}).order_by([:order, :desc])

      map = {}
      @preview_items.each do |item|
        order = item['order']
        map[order] = [] if map[order] == nil
        map[order].push(item)
      end

      @json_items = []
      map.keys.each do |key|
        arr = map[key]
        if arr.count == 1
          @json_items.push(arr[0])
        else
          @json_items.push(arr[rand(arr.count)])
        end
      end
      render json: @json_items, callback: params[:callback]
    end
```



rails某个模型的index页面，具体做了什么呢？大概是四件事情：






  1. 取参数version


  2. 在数据库查询该version并且order字段大于0的数据，根据order字段倒叙排列（数据库是mongodb，driver用mongoid）


  3. 遍历查询的结果建立一张以order为key，相应order的实体数组为value的表


  4. 遍历3中的表，每个order的数组中随机抽取一个元素作为这个order的唯一值




最后把获取的新数据render。




看起来很满足什么的，就提交代码了。之后同事部署前，看了看这里的更新，然后最终把代码完全改掉了。只用了一行 =。=



```ruby
    def index
      @json_items = MyItem.where({:version => params[:version] || 'i', :order.gt => 0}).order_by([:order, :desc])
        .group_by(&:order).map {|k,v| v.sample}
      render json: @json_items, callback: params[:callback]
    end
```




做的事情是完全一样的。






  1. 他先看了取参数，不用三元，直接 || 一个默认值


  2. 然后他把第4步中的取随机的部分改了，因为数组中随机取一个元素，ruby直接有sample方法


  3. 然后他又觉得第四步可以更简单的调用hash的map方法，映射成另一个value = value.sample的map


  4. 最后他吃了顿中饭回来灵光一闪，终于发现第三步做的就是一个group_by的事情，ruby可以做这个事情




于是，除了查询不变，所有的逻辑全部调用了ruby现成的方法完成了！哥落伍了…… 越发觉得ruby好强大。




BTW，帮我改代码的是@niedhui




以上……



