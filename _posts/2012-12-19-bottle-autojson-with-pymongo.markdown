---
layout: post
slug: bottle-autojson-with-pymongo
title: "bottle直接返回pymongo查询结果"
date: 2012-12-19 10:32
comments: true
categories:
- Programing
tags:
- pymongo
- mongodb
- python
- bottle
- json
- bson
---

以前提到过[bottle](/bottle-i-love-this-framework/)，也写过在[django里使用pymongo](/use-mongodb-with-django/)，这次是在bottle里用pymongo。

bottle有类似ROR的一些特性，比如处理请求的时候直接return一个字典，框架会自动把它parse成json（autojson）。

我是想偷个懒来着，把代码写成了下面这样。

```py
@get('/api/today')
def api_today():
    udid = request.GET.get('udid')
    return self.coll.find_one({'udid':udid, 'date':today})
```

这会有问题，因为find_one返回的bson没办法直接parse。伟大的发明都是在偷懒的时候诞生的，看起来我要做的事情也很简单，只要能让我改一改parser就行了。

1. 写一个dump方法替换原来JSONPlugin里的
2. 设置Bottle app的autojson为False。`Bottle(autojson=False)`
3. 把有自己dump方法的JSONPlugin给install到app里

```py
class MongoEncoder(JSONEncoder):
    def mongo_dumps(obj):
        # convert all iterables to lists
        if hasattr(obj, '__iter__'):
            return list(obj)
        # convert cursors to lists
        elif isinstance(obj, pymongo.cursor.Cursor):
            return list(obj)
        # convert ObjectId to string
        elif isinstance(obj, bson.objectid.ObjectId):
            return unicode(obj)
        # dereference DBRef
        elif isinstance(obj, bson.dbref.DBRef):
            return db.dereference(obj) # db is the incetance database
        # convert dates to strings
        elif isinstance(obj, datetime.datetime) or isinstance(obj, datetime.date) or isinstance(obj, datetime.time):
            return unicode(obj)
        return json.JSONEncoder.default(self, obj)

app = app()
app.autojson = False
m_encoder = MongoEncoder()
app.install(JSONPlugin(json_dumps=lambda s: dumps(s, default=m_encoder.mongo_dumps)))
```

via:

 * [https://github.com/defnull/bottle/issues/287](https://github.com/defnull/bottle/issues/287)
 * [https://gist.github.com/2779820](https://gist.github.com/2779820)