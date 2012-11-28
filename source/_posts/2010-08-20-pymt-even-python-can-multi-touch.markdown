---
comments: true
date: 2010-08-20 00:38:24
layout: post
slug: pymt-even-python-can-multi-touch
title: PyMT,小蟒蛇也懂多点触摸
wordpress_id: 568
categories:
- Programing
tags:
- multi-touch
- open source
- Python
---

相信Windows7已经有很多人在用了，应该说比起vista是好很多了。Win7有个很大的新特性是支持多点触摸，当然到目前为止我还没有条件使用多点触摸！不过这并不影响人们对多点触摸的热情，Apple除了Magic Trackpad，磨刀霍霍向鼠标，看来触控操作将是趋势。开发者也怀着满腔热情的在做一些multitouch的轮子，比如PyMT，就是python支持多点触控的一个库。更重要的是，它是跨平台的~




[![](/images/uploads/wp/pymt_logo.png)](http://pymt.eu/)




PyMT是一个开源的类库，支持多点触控。在[这里](http://pymt.eu/#download)可以下载到三个平台的版本，github上host了[源代码](http://github.com/tito/pymt)。我下载了Windows7下面的版本来看看，解压后有192MB的大小，看起来料很足的样子，其实它完整的下了一个python，真正pymt本身只有20多M，不过也很大了。解压缩得到的那个pymt.bat批处理文件，就可以来运行pymt的程序了。如果你迫不及待想试试看，并且你有多点触摸的设备（没有也可以啦），那么有个简单的办法。




在命令行输入：




    pymt -m pymt.tools.demo




之后就可以在弹出的窗口里尝试一下多点触摸了。




[![](/images/uploads/wp/2010-08-20_pymt_demo.png)](/images/uploads/wp/2010-08-20_pymt_demo.png)




利用类库也不难，据说最简单的代码只需要两行。




    from pymt import *

    class CircleDrawer(MTWidget):
        '''Draw a circle at the position of all touches.'''
        def draw(self):
            set_color(1, 0, 0)
            for touch in getCurrentTouches():
                drawCircle(touch.pos, 50)

    runTouchApp(CircleDrawer())




简单介绍到这里。如果你的应用程序有对多点触摸的需求，不妨试试看用pymt来写？




以上。
