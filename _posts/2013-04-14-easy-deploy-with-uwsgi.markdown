---
layout: post
title: "快速使用uwsgi部署"
date: 2013-04-14 01:02
comments: true
categories:
- Programing
tags:
- python
- bottle.py
- ubuntu
- uwsgi
- nginx
---

之前尝试在阿里云部署一个bottle.py写的web服务，选择了unbuntu12.04，一穷二白的ubuntu上用uwsgi部署还挺简单的。这里简单记录一下。


先要改一改程序代码，声明一个application

```py
application = bottle.app()
```

然后安装uwsgi，demo.py就是修改过的bottle的代码。

这里我遇到了`— unavailable modifier requested: 0 --`，要安装uwsgi-plugin-python，并且uwsgi启动时候添加`--plugin python`

一些uwsgi的操作看[这里](http://uwsgi-docs.readthedocs.org/en/latest/Management.html)

```sh
apt-get install uwsgi uwsgi-plugin-python
uwsgi --socket :8000 --plugin python --file demo.py --processes 4 --pidfile /tmp/demo.pid --touch-reload=/tmp/restart -d uwsgi.log
```

安装nginx

```sh
apt-get install libpcre3 libpcre3-dbg libpcre3-dev
apt-get install zlib1g zlib1g-dbg zlib1g-dev
apt-get install make
cd /path/to/nginx
./configure
make
make install
```

最后配置nginx。默认安装路径在`/usr/local/nginx/conf/nginx.conf`

```js
server {
    listen       80;
    server_name  demo.artori.us;

    location /static/ { alias /home/root/demo/static/; }

    location / {
        uwsgi_pass      127.0.0.1:8000;
        include         uwsgi_params;
    }
}
```

自此搞定

via:

 * [http://stackoverflow.com/questions/10748108/nginx-uwsgi-unavailable-modifier-requested-0](http://stackoverflow.com/questions/10748108/nginx-uwsgi-unavailable-modifier-requested-0)
 * [http://lists.unbit.it/pipermail/uwsgi/2011-November/002923.html](http://lists.unbit.it/pipermail/uwsgi/2011-November/002923.html)
 * [http://aaronsnow.tumblr.com/post/11560674160/nginx-uwsgi-bottle-py](http://aaronsnow.tumblr.com/post/11560674160/nginx-uwsgi-bottle-py)
 * [https://groups.google.com/forum/?fromgroups=#!topic/bottlepy/wRfgm4obLXk](https://groups.google.com/forum/?fromgroups=#!topic/bottlepy/wRfgm4obLXk)
 * [http://blog.felixc.at/2011/01/ubuntu-uwsgi-nginx-bottle-configuration/](http://blog.felixc.at/2011/01/ubuntu-uwsgi-nginx-bottle-configuration/)
 * [http://apt-blog.net/moinmoin-on-nginx-via-fastcgi-and-uwgi](http://apt-blog.net/moinmoin-on-nginx-via-fastcgi-and-uwgi)
 * [http://down.chinaz.com/server/201112/1467_1.htm](http://down.chinaz.com/server/201112/1467_1.htm)
