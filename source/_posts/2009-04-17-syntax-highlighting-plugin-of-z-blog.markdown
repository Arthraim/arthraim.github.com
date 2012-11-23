---
comments: true
date: 2009-04-17 23:54:46
layout: post
slug: syntax-highlighting-plugin-of-z-blog
title: Z-blog的Syntax Highlighting插件
wordpress_id: 7
categories:
- Programing
tags:
- 语法高亮
- Hello World
- Z-blog
---




一直觉得Z-blog很好，就是每次想写一些技术文，就会觉得哎呀没有代码插入功能好麻烦。于是今天终于下定决心好好找一个，哪怕没有也要自己写一个，哪怕不是插件，根目录下面放一个页面自己转换转换也好啊。




在寻觅了很久之后，发现其实Z-blog就有这样的插件，叫做CodeLight，出自[这位](http://www.macgoo.com/myblog/)博主之手。[这是](http://bbs.rainbowsoft.org/thread-30667-1-1.html)发在[rainbowsoft.org论坛](http://bbs.rainbowsoft.org/)上的贴，有人提意见建议，有人去感谢作者，也有人交流如何继续发展下去，感觉不错。对了，还有最重要的，作者的[官方发布页](http://www.macgoo.com/myblog/archives/102/)，像是每个版本作者都在那里更新了，下载地址也去那里找吧，所谓饮水思源，我就赤裸裸的拉个下载链接过来可不好。




下面我就乱贴一些代码吧测试一下吧，貌似不用他们那样的测试，我就谢谢hello world，估计今天将是写hello world写的最多的一天了。



    
    #include <iostream>
    using namespace std;
    int main()
    {
        cout<<"Hello wolrd!"<<endl;
        return 0;
    }
    






    
    #include<stdio.h>
    int main()
    {
        printf("Hello wolrd!n");
        return 0;
    }
    






    
    using System;
    namespace HelloWorld
    {
        class Program
        {
            public static void Main(string[] args)
            {
                Console.WriteLine("Hello World!");
                Console.ReadKey(true);
            }
        }
    }
    






    
    public class HelloWorld
    {
        public static void main(String[] args)
        {
            System.out.println("Hello World!");
        }
    }
    










就来个C家族的大会面吧，其他也都不太会，呵呵。挺好玩的，感谢作者，以后可以悄悄的发一些代码了。




另外，之前老弟来说回复不上来，然后才想起来那天网速慢于是插件没有弄好（弄了一半），也就是说第二天到今天以来，访客都不能正常回复，我现在搞定这个问题了～ 嘿嘿
