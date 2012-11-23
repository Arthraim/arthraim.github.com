---
comments: true
date: 2010-11-20 01:42:17
layout: post
slug: 5-technology-trends-programmers-should-know
title: 程序员应该知道的5个技术发展趋势
wordpress_id: 652
categories:
- Programing
tags:
- agile methods
- dynamic
- functional programming
- git
- mongodb
- no-sql
- Python
- ruby
---

首先声明这个文字的内容是在阮一峰的英文博客上看到的，先把原文贴过来吧：




> 
	
> 
> 5 Technology Trends Programmers Should Know  

		by ruanyf
> 
> 
	
> 
> 
		
>   * **Learn and use a modern scripting language**. Ruby, Python, Groovy… What matters is having a quick and easy tool at hand for anything.
> 
		
>   * **Learn and use a modern version control system**. Git or Mercurial.
> 
		
>   * **Be familiar with NoSQL solutions**, like MongoDB and CouchDB. They works when traditional relational DBs reach their limits at scaling and performance.
> 
		
>   * **Learn a functional language — or more than one**. Erlang, Haskell or OCaml.
> 
		
>   * **Study agile methods and concepts**. Agile helps to standardize management and daily programmer work, enforcing a small, controllable devel/release/testing cycle and also encouraging good communication all across the team.
> 
	
	
> 
> via:[  

		http://blog.mostof.it/top-5-trends-in-software-development/](http://blog.mostof.it/top-5-trends-in-software-development/)  

		[http://www.ruanyifeng.com/en/2010/11/5-technology-trends-programmers-should-know/](http://www.ruanyifeng.com/en/2010/11/5-technology-trends-programmers-should-know/)
> 
> 





后面是我大概说说，说说自己对它们的了解，欢迎大家一起讨论~




**动态语言**：python, ruby是难分伯仲的两种动态脚本语言，我个人更喜欢python的语法，甚至有人说python是厌倦了括号甚至shift的人用的。ruby其实是最接近lisp的，所以在我的认知里，ruby的底层实现是强于python的，另外rails也似乎比无论是django还是pylons要强大的多。groovy最大的特点就是它是基于JVM的，所以无论是JVM带来的优点还是缺点都将成为选择这个语言的主要因素。当然还有很多动态语言，比如在WEB前端呼风唤雨的javascript，还有actionscript。极度接近模板语言的php，还有在游戏UI领域应用比较广泛的lua语言。别忘了还有老大哥LISP。




**版本控制**：CVS,SVN并不在这个modern的范围里。这里我只使用过git，因为github的关系才会接触它（github现在也支持svn了），不过我实在是说不出来git的真正优势，个人也刚刚会用而已，从SVN过来还有很多不喜欢。比如git pull是git fetch + git merge，要untrack一个文件的话要用git remove --cache。git必须在本地创建仓库的特性让提交都在本地完成，之后再push到远程服务器。和SVN一样，只有真正用过了恐怕才会遇到各种问题熟悉这个东西吧，一个人自己commit,push岂不是很寂寞……




**NO-SQL**：文档型数据库因为灵活快速，迅速流行了起来。虽然关于它并不能说完全能取代传统关系型数据库，但它具有的一些特性真的是让人爱不释手。比如在我实用中非常受用的灵活的结构，如果要alter表结构，在mongodb里只要给新数据直接加上新字段，老数据在必要的时候update就可以了。另外update如果where不存在，也会新建记录。新的collection在第一次使用的时候就会产生，完全不必create table（初学的时候我还在努力寻找create collection）。当然mongodb的查询速度也是非常的令人愉快，规模不是很大的话甚至不需要使用memcached。当然我眼里的no-sql数据库有两个巨大的劣势：因为没有inner join，所以设计的时候很多数据可能要被冗余，在这一点上完全不是关系数据库的那套设计哲学；还有一个是一些高级的统计，通过map/reduce才能完成，很多讨论认为这是NO-SQL的最大的倒退，这种依赖程序员的算法的查询可能会让开发效率运行效率都极端低下。




**函数式编程语言**：其实函数是编程很难理解，更像是一种思想，至今我的理解也似乎只是不太明白。例子有很多，Haskell, Erlang，scala，或者是微软的F#。函数式编程中数据是不能被改变的，函数计算得到的数据都应该是新的数据，有点像C#中的readonly和java中的final声明的变量。当然函数式编程思想正在被其他的一些语言所吸收。




**敏捷方法**：敏捷这个东西天天听到天天看到，但是让我说是什么样的东西我还真说不出来。敏捷方法旨在解决大型软件开发中应对快速变化的需求而产生的开发迭代问题。在具体使用中有很多实际的做法，比如开发未动测试先行之类的。这个概念对我来说也还很抽象，一起学习吧。




不唠叨了，就这样吧……  欢迎拍砖~




以上。
