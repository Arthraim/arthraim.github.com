---
comments: true
date: 2010-09-29 01:43:13
layout: post
slug: pymongo-errors-autoreconnect-could-not-find-masterprimary
title: 'pymongo.errors.AutoReconnect: could not find master/primary'
wordpress_id: 593
categories:
- Programing
tags:
- mongodb
- pymongo
- Python
---

不说前因后果了, 在使用pymongo连接服务器mongodb的时候遇到了pymongo.errors.AutoReconnect: could not find master/primary这个错误. 折腾了很久才发现是个简单的问题, 不过鉴于我Google了很久也没有找到合适的答案, 不妨在博客记录一下, 我的问题说不定是将来你遇到的问题.




具体错误如下:



    
    Traceback (most recent call last):
      File "/home/arthur/workspace/DUMMY/python/DUMMY.py", line 7, in <module>
        mongo_con = pymongo.Connection( )
      File "/usr/local/lib/python2.6/dist-packages/pymongo-1.8.1-py2.6-linux-x86_64.egg/pymongo/connection.py", line 291, in __init__
        self.__find_master()
      File "/usr/local/lib/python2.6/dist-packages/pymongo-1.8.1-py2.6-linux-x86_64.egg/pymongo/connection.py", line 495, in __find_master
        raise AutoReconnect("could not find master/primary")
    pymongo.errors.AutoReconnect: could not find master/primary</module>




引起这个错误的原因有很多, 比如mongod根本没有起来, Google到还有早期版本可能存在的bug. 我连接任何一台计算机上的mongodb都是正常的, 唯独连接那台的有问题. 在我闷头升级服务器的python版本(2.7), 升级了服务器的mongodb版本(1.8.1), 都于事无补(事实上在我的尝试看来, pymongo 1.8.1连接1.4.3以上的mongodb都没有问题).




最后问题是因为Master-Slave. 因为升级mongo的时候要更新各个节点, 之后同事重启后才突然想到, 连不上是因为这台服务器是slave. 找会不会是这个原因引发的问题的时候, 在[这里](http://api.mongodb.org/python/1.7/api/pymongo/master_slave_connection.html)仔细一看发现构建Connection的时候, 有一个slave_okay的参数, 设置为True之后就可以单独连接一个slave节点了. 当然文档里pymongo.master_slave_connection.MasterSlaveConnection类就更重了.



    
    mongo_con = pymongo.Connection(slave_okay=True)
    




最后随便说一句, mongodb真的是个好东西, [人间网](http://renjian.com)的技术人员从我来之前就一直尝试使用它.




以上.



