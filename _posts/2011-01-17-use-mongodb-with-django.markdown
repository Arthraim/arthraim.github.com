---
comments: true
date: 2011-01-17 19:31:47
layout: post
slug: use-mongodb-with-django
title: 优雅的在django框架里使用mongodb
wordpress_id: 724
categories:
- Programing
tags:
- django
- mongodb
- open source
- pymongo
- Python
---

在我们这里关于ruby和python的争论永远没有停息, 比赛之前也无意间让我发现了很多东西. 这次发现了一个django中使用mongodb的好东西, 叫做[mongoengine](http://mongoengine.org/), 不知道是不是我火星了, 因为从[github](https://github.com/hmarr/mongoengine)上看这个项目最早从09年11月就开始了.




在github下载到源码, 有setup.py, 先build再install, 然后... 开搞!




先很简单的创建一个django的工程(具体不说django), 然后弄个小app或者随便哪里写个view就好了. 然后我用了几步就确定它可以正常使用了.




首先修改settings.py, 原来DATABASES完全不用去管它了, 全部设为空串就好, 然后在文件里加上下面的内容

```py
    from mongoengine import connect
    connect('DB_NAME')
```



在models.py里随便写个模型, 这里要用到mongoengine的一些内容



```py
from mongoengine import Document

class TestModel(Document):
    test_key = StringField(required=True)
    test_value = StringField(max_length=50)
```



在某个views.py里随便哪里写点逻辑, 添加条数据而已(两种方式都可以填数据)



```py
    entry = TestModel(test_key='arthur')
    entry.test_value = 'Wang'
    entry.save()
```




然后就可以看看数据输出啦



```py
    for entry in TestModel.objects:
        print entry.test_key

```


好吧, 如果顺利就应该可以看到console输出的结果, 很给力. 当然在mongo中可以查到如下结果


```sh
db.testmodel.find()
{ "_id" : ObjectId("4d34267f7ecfdb3b7c000000"), "test_key" : "arthur", "test_value" : "Wang", "_types" : [ "TestModel" ], "_cls" : "TestModel" }
```



挺好玩的. 最重要的是它支持sessions, 支持User authentication, 还可以使用gridfs做文件存储, 具体可以在[这里](http://mongoengine.org/docs/v0.4/django.html)查到.



