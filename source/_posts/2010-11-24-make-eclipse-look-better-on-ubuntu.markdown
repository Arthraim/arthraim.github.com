---
comments: true
date: 2010-11-24 19:44:42
layout: post
slug: make-eclipse-look-better-on-ubuntu
title: 让eclipse在ubuntu下面好看一点
wordpress_id: 657
categories:
- O.S.
tags:
- eclipse
- gnome
- Linux
- Ubuntu
---

谨以此文献给所有小气鬼.




话说ubuntu下面(其他gnome的估计差不多, 不知道kde是什么光景) 的eclipse真是难看的爆掉, 工具栏高的可以占掉全世界了, 弄得这么高干吗啊, 还有各种标签各种XX各种OO. 本本的分辨率是1280*800, 这样的margin真叫人难以忍受. 虽然19寸的1680*1050多少应该空间宽裕一点, 但是小气鬼还是觉得非常的不给力, 受不了它的高度宽度温度湿度经度纬度!




于是我生气了, 于是我在寻求了别人帮助未果然后憋屈了好多年之后我终于生气了. 于是我今天去Google了, 于是我Google到了, 顺便提一下另外一个很让小气鬼生气的事情是Google到的结果是要翻墙才能看到的. 于是我在墙外某处看到了一个灰常牛B的做法, 自己改.gtkrc文件.




在用户目录下(就是.bashrc的目录) 修改或创建 .gtkrc 文件, 然后填上以下内容.




    style "gtkcompact" {
    font_name="Sans 8"
    GtkButton::default_border={0,0,0,0}
    GtkButton::default_outside_border={0,0,0,0}
    GtkButtonBox::child_min_width=0
    GtkButtonBox::child_min_heigth=0
    GtkButtonBox::child_internal_pad_x=0
    GtkButtonBox::child_internal_pad_y=0
    GtkMenu::vertical-padding=1
    GtkMenuBar::internal_padding=0
    GtkMenuItem::horizontal_padding=4
    GtkToolbar::internal-padding=0
    GtkToolbar::space-size=0
    GtkOptionMenu::indicator_size=0
    GtkOptionMenu::indicator_spacing=0
    GtkPaned::handle_size=4
    GtkRange::trough_border=0
    GtkRange::stepper_spacing=0
    GtkScale::value_spacing=0
    GtkScrolledWindow::scrollbar_spacing=0
    GtkExpander::expander_size=10
    GtkExpander::expander_spacing=0
    GtkTreeView::vertical-separator=0
    GtkTreeView::horizontal-separator=0
    GtkTreeView::expander-size=8
    GtkTreeView::fixed-height-mode=TRUE
    GtkWidget::focus_padding=0
    }
    class "GtkWidget" style "gtkcompact"

    style "gtkcompactextra" {
    xthickness=0
    ythickness=0
    }
    class "GtkButton" style "gtkcompactextra"
    class "GtkToolbar" style "gtkcompactextra"
    class "GtkPaned" style "gtkcompactextra"

说实话小气鬼真的看不懂, 也懒得看懂, 反正eclipse现在不碍着我了, 缺点是没有碍着我的其他软件其他界面里的一些布局都发生了一些变化, 不过这种让出屏幕空间的变化对一般人来说是缺点, 但是对小气鬼来说也不能不说是另一种feature~!  最后小气鬼心满意足的放一张截图~~ 这窄窄的看上去多顺眼啊~




[![](/images/uploads/wp/2010-11-24_better_look_eclipse.png)](/images/uploads/wp/2010-11-24_better_look_eclipse.png)




via: [http://lj4newbies.blogspot.com/2008/02/make-your-eclipse-look-better-on-ubuntu.html](http://lj4newbies.blogspot.com/2008/02/make-your-eclipse-look-better-on-ubuntu.html)




以上!
