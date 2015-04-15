---
comments: true
date: 2009-05-13 23:25:35
layout: post
slug: microsoft-axum-learn-grammar-with-video
title: Microsoft Axum —— 跟着视频看语法(AxumMath的实现)
wordpress_id: 32
categories:
- Programing
tags:
- .net
- Axum
- Microsoft
---




其实这是一种抄袭行为，跟着主页上的视频把代码实现了一遍，用以理解所谓的 ：




>

>
> "A .NET language for safe, scalable and productive parallel programming through isolation, actors and message-passing."
>
>





平台为Windows 7 build 7057（没有更新RC版啊），IDE是Visual Studio 2008。<strike>不知道2005是不是支持，我的windows xp中安装了Axum和VS2005的，一会考证一下。</strike> 经确认VS2005是不能新建Axum工程的。




**下图为IDE和程序运行结果。**




![](/images/uploads/zb/AxumMath.PNG)




虽然实现的程序和C#的ReadLine获得几个数字，加一下乘一下得到的效果是一样的，不过我的数据不是通过访问内存的，而是通过Axum提供的数据传输方式的。




简单的说：首先做了一个channel，作为一切传输的通道，输入的数据是int X和int Y，输出的数据是int Z。然后在这个channel的基础上新建两个agent，分别完成相加和相减的过程。




>

>
> Z <-- ( receive(X) + receive(Y))
>
>





这句话是核心，receive(X)的方法，是agent从channel中接受X的信息，而做出处理后 <-- 传输给Z，Z就得到了需要的结果。（究竟是先处理数据再传输，还是传输过来后再处理不得而知）




UI逻辑中，首先将两个agent创建不同的domain（话说Axum是弱类型语言呢～），然后就可以通过ReadLine得到输入的数据，通过之前创建的domain直接去接收结果。




这是单击完成的，不知道分布之后是不是有特殊地方。下面是视频中整个程序的结构图例。







![](/images/uploads/zb/2009-05-13_frame.png)




**下面插入代码，（看来是不是应该自定义高亮方式了呢？先采用相对接近的C#吧～）**




    using System;
    using System.Concurrency;
    using Microsoft.Axum;
    namespace AxumMath
    {
        channel IntOperater
        {
            input int X;
            input int Y;
            output int Z;
        }
        private agent AdderAgent : channel IntOperater
        {
            public AdderAgent()
            {
                while(true)
                {
                    Z <-- ( receive(X) + receive(Y));
                }
            }
        }
        private agent MultiplierAgent : channel IntOperater
        {
            public MultiplierAgent()
            {
                while(true)
                {
                    Z <-- ( receive(X) * receive(Y));
                }
            }
        }
        private agent MainAgent : channel Application
        {
            public MainAgent()
            {
                var adder = AdderAgent.CreateInNewDomain();
                var multiplier = MultiplierAgent.CreateInNewDomain();
                while ( true )
                {
                    Console.Write("X: ");
                    var x = Int32.Parse(Console.ReadLine());
                    Console.Write("Y: ");
                    var y = Int32.Parse(Console.ReadLine());
                    adder::X <-- x;
                    adder::Y <-- y;
                    multiplier::X <-- x;
                    multiplier::Y <-- y;
                    Console.WriteLine( "Sum: " + receive(adder::Z ));
                    Console.WriteLine( "Product: " + receive(multiplier::Z ));
                }
            }
        }
    }




