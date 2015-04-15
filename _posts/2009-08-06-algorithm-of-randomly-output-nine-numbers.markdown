---
comments: true
date: 2009-08-06 14:29:36
layout: post
slug: algorithm-of-randomly-output-nine-numbers
title: C#随机顺序输出数字 1～9 的算法
wordpress_id: 90
categories:
- Programing
tags:
- Algorithm
- C#
---







我很少关注算法，虽然之前也参加过ACM ICPC的省赛，也参加过一年半载的集训，但是我还是对算法这个东西提不起精神。我觉得那是数学家们站在前线做的事情，而我们大多数时间是在使用而不是创造，相比较之下反而是其他的一些事情比较有创造的快感。也许创造算法的人是很有快感吧，不过毕竟我只是个小小的程序员而已。这次的算法听上去很简单。以前在一些程序里常用到的随机数都是没什么特别的，昨天偶然发现要让1-9 九个数字随机乱序输出也是一件不容易的事情。




一上来的想法很简单，随机生成一个数字，然后检查是不是存在了，存在了就无视，重新生成。于是反例代码在这里：



    
    
    public List<int> buff = new List<int>();
    Random _random;public MessUp()
    {
    long tick = DateTime.Now.Ticks;
    _random = new Random(DateTime.Now.Millisecond);
    int r;    while ((r = _random.Next(1, 9)) > 0)
    {
    if (Exists(r) == false && buff.Count != 9)
    {
    buff.Add(r);
    }
    else if (buff.Count == 9)
    break;
    }
    }
    private bool Exists(int num)
    {
    foreach(int i in buff)
    {
    if (i == num)
    return true;
    }
    return false;
    }




乍看之下就是这样，轻松愉快的事情。不过实际运行一下就知道效果了。123456789，9个数，比如等我随机出来12345678之后再要随机出来一个9这概率就变得非常可怕了。后来看到一个帖子，说到一句话："随机出一个后，排除。" 多简单的思路啊，要是能写成这样就好了。



    
    
    int newNumber = _random.Next(0,previousMin,previousMax,10);




边界不取到，min和max自己用newNumber比出来并记录，做个美梦而已。排除中间，不如把中间的数放到末尾再排除。算法的代码如下：



    
    
    static void Main(string[] args)
    {
    List<int> tempL = new List<int>();
    List<int> buff = new List<int>();
    for (int i = 1; i <= 9; i++)
    tempL.Add(i);
    int len = tempL.Count;
    Random _random = new Random(DateTime.Now.Millisecond);
    for (int i = 0; i < len; i++)
    {
    int r = _random.Next(len - i);
    buff.Add(tempL[r]);
    tempL[r] = tempL[len - 1 - i];
    }
    foreach (int i in buff)
    Console.Write(i);
    Console.WriteLine();
    }




先准备一个List，我命名为tempL记录1-9，然后每一次随机生成一个索引，也就是位置的数字。把那个位置放到我们要记录的另一个List中，我命名为buff。并且把最后一个数放回到被抽取的数字的位置上。第n次循环，随机生成位置数要排除最后n-1个位置。
