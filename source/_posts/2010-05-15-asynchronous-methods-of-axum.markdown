---
comments: true
date: 2010-05-15 13:13:04
layout: post
slug: asynchronous-methods-of-axum
title: Axum的异步方法
wordpress_id: 428
categories:
- Programing
tags:
- .net
- Axum
- 异步
- Microsoft
---

有的事情一拖就被我拖了一年，一年前我想把Axum的UserGuide讲讲完，后来就一直没有做了。今天突然翻了一下以前的文章，发现是去年5月1X号的时候在关注的东西。于是我回去看了一看Axum在这一年里做了些什么。在去年7月份，Axum就公布了0.2版本，修复了一些bug。其中最让我在意的，就是异步方法。




这些异步方法是个什么情况呢，[官方博客](http://blogs.msdn.com/maestroteam/archive/2009/05/23/yielding-with-asynchronous-methods.aspx)上有个例子很好的说明了这个问题，我拿来用一下。这是一个简单的Axum程序。



    
    agent MainAgent : channel Microsoft.Axum.Application
    {
        public MainAgent()
        {
            var pt = new OrderedInteractionPoint<int>();
    
            // Set up a dataflow network
            pt ==> MultiplyByTwo ==> Print;
    
            // Send some numbers to the network
            for(int i=0; i<5; i++) pt <-- i;
    
            PrimaryChannel::Done <-- Signal.Value;
        }
    
        int MultiplyByTwo(int n)
        {
            return n*2;
        }
    
        void Print(int n)
        {
            Console.WriteLine(n);
        }
    }




按照这个代码执行的结果应该是怎么样的呢？好吧，其实什么都不会输出。不难理解，整个pipeline是去其他线程跑的，主线程呢？直接来了个Done，于是其它东西还没出结果，Done信号已经来了，程序被结束了。解决方法呢？那加个Readline好不好？



    
    // Send some numbers to the network
    for(int i=0; i<5; i++) pt <-- i;
    
    // Not so fast! Wait until the user hits
    // return before continuing
    Console.WriteLine("Press Enter to continue...");
    Console.ReadLine();
    
    PrimaryChannel::Done <-- Signal.Value;




绝对脑残啊！我相信按道理很多人会想到这么写，不过在把代码敲出来之前肯定就想到了问题。我的Console IO被阻塞了说，还怎么去Writeline啊。好吧，Axum 2.0里的一个很重要的更新就是给了一个异步的Console：



    
    // Send some numbers to the network
    for(int i=0; i<5; i++) pt <-- i;
    
    // Not so fast! Wait until the user hits
    // return before continuing
    Console.WriteLine("Press Enter to continue...");
    AsyncConsole.ReadLine();
    
    PrimaryChannel::Done <-- Signal.Value;




这真是个好东西啊！具体Axum0.2还更新了什么，看看下面的。其实0.3也有了，只是我只是想知道一些0.1到现在的变化，所以先写这个吧~ 另外0.2的时候还发了一个独立的Axum编译器，不依赖Visual Studio，有兴趣的童鞋们可以更加轻松愉快的玩一玩了~




0.2的主要更新。





	
  * Added an installer for Visual Studio 2010 Beta1

	
  * Enabled parallel execution of functional nodes in dataflow networks

	
  * Made it possible to change fonts and colors of Axum language elements via Tools | Options | Fonts and Colors

	
  * Moved samples to a zip file to make using them easier

	
  * Introduced AxumLite.zip - an Axum command line compiler that doesn't require Visual Studio

	
  * Fixed the compiler error where the channel name was the same as the enclosing namespace name

	
  * Made handling of immutable primitive types more rigorous; fixed some side-effect related bugs

	
  * Added 'using System.Concurrency.Messaging' to the VS-generated template to make classes like OrderedInteractionPoint visible by default

	
  * Added the async method Microsoft.Axum.IO.Console.ReadLine

	
  * Added a spiffy Auction sample (A big shout out to [Matthew Podwysocki](http://weblogs.asp.net/Podwysocki/) for his help!)




以上~
