---
layout: post
title: "bottle.py实现websocket应用"
date: 2014-02-11 09:15:50 +0800
comments: true
categories:
- Programing
tags:
- python
- bottle.py
- websocket
- gunicorn
- gevent
---

之前实现了一个websocket的server，用到gevent，尝试用uwsgi部署几经受挫，最后用gunicorn成功部署到生产环境了。

决定简单总结一下

## bottle-websocket

虽然可以直接用gevent啥的封装，不过这里简单用到一个库，[bottle-websocket](https://github.com/zeekay/bottle-websocket) ，帮助把gevent-websocket、gevent都封装了，看了下实现，没几行代码，一是添加了`run`的时候的一个server `GeventWebSocketServer`，另一个是request用的`websocket`插件，方便许多。

## 举个栗子

简单写了一个例子，发现和bottle-websocket的example里的chat类似。

```python
from bottle import *
from bottle.ext.websocket import GeventWebSocketServer
from bottle.ext.websocket import websocket

ws_set = set()

@get('/')
def idx():
    return """
    <!DOCTYPE html>
    <html>
      <head>
        <title>title</title>
        <meta http-equiv="content-type" content="text/html; charset=UTF-8">
        <script src="//code.jquery.com/jquery-1.10.2.min.js" type="text/javascript"></script>
      </head>
      <body>
        <form id="send">
            <input id="input" type="text" />
            <input type="submit" />
        </form>
        <ul id="box"></ul>
        <script>
            // websocket connection
            ws = new WebSocket('ws://127.0.0.1:8001/websocket');
            ws.onopen = function(evt) {
                console.log('connected');
            }
            ws.onmessage = function(evt) {
                console.log('[GOT]' + evt.data);
                $("#box").append("<li>" + evt.data + "</li>");
            }
            function sendToSocket(msg) {
                return ws.send(msg);
            }
            // send
            $("#send").submit(function () {
                var msg = $("#input").val();
                sendToSocket(msg);
                return false;
            });
        </script>
      </body>
    </html>
    """

@get('/websocket', apply=[websocket])
def chat(ws):
    ws_set.add(ws)
    while True:
        msg = ws.receive()
        if msg is not None:
            for u in ws_set:
                u.send(msg)
        else: break
    ws_set.remove(ws)

if __name__ == '__main__':
    run(host='127.0.0.1', port=8001, server=GeventWebSocketServer)

```

保存到文件bottle_websocket_app.py，在终端运行这个脚本，然后多打开几个浏览器tab，访问127.0.0.1:8001就能试试效果了。

## 使用gunicorn部署

说到生产环境用了gunicorn部署，`pip install gunicorn`安装gunicorn，

还是和uwsgi保持一致，在刚才文件的后面，添加一行内容。

```python
# ...

application = app()

```

直接在sh中使用下面的命令启动gunicorn

```
gunicorn bottle_websocket_app:application -b 127.0.0.1:8001 -w 1 --worker-class "gevent" -k "geventwebsocket.gunicorn.workers.GeventWebSocketWorker" --access-logfile access.log --error-logfile error.log --daemon
# 好长的一行 ¬_¬
```

如果你和我一样用到nginx，简单配置成下面这样子就可以了

```javascript
server {
    listen       80;
    server_name  ws.artori.us;
    location / {
        proxy_pass http://127.0.0.1:8001;
        proxy_http_version 1.1;
    }
}

```

胡乱记录之
