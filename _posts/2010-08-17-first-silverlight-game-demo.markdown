---
comments: true
date: 2010-08-17 23:30:44
layout: post
slug: first-silverlight-game-demo
title: 第一次用Silverlight写游戏demo
wordpress_id: 563
categories:
- Programing
tags:
- demo
- Silverlight
- 游戏开发
---

话说本文4个背景。上一篇说装了XNA4.0的环境，于是还水了一篇玩模拟器的文，总觉得XNA不给力，于是想试试看silverlight写游戏会怎么样，上星期花了两个晚上试了一试，以上是第一个背景。我对游戏总是念念不忘，算是自己的梦想吧。以前写的XNA教程里从来没有提过动画的事情。最后一个原因，就是我好久没有更新博客了。




话说这个东东是按照cnblogs上注明的MVP[深蓝色右手](http://www.cnblogs.com/alamiye010/)的教程写的，如果你要学习，那也许他的东西更好吧。我是很给力的想给自己的demo加上一点骚包的东西。看看实际的效果吧要不~




要不demo还是不直接贴在文章里了，跳转一下看吧：[http://artorius.arthraim.com/page/sl1/](http://arthraim.cn/page/sl1/)




特别留意林克的眨眼，给眨眼做了个特殊的序列来实现很多重复帧的动画。另外固定的是移动时间，所以移动起来很傻吧，看着玩玩好了，貌似还有bug我三更半夜的搞得有点头大，乱的一塌糊涂，有心的谁来帮我看看？然后那啥，贴下代码吧~



```c#
public partial class MainPage : UserControl
{
    Storyboard storyboard;
    Image spirit;
    int count = 1;
    Point relativePos = new Point(13, 23);
    int direction = 1;
    bool isWalking = false;

    public MainPage()
    {
        InitializeComponent();

        spirit = new System.Windows.Controls.Image();
        spirit.Stretch = Stretch.Uniform;
        spirit.Width = 23;
        spirit.Height = 23;
        Carrier.Children.Add(spirit);
        Canvas.SetLeft(spirit, 0);
        Canvas.SetTop(spirit, 0);

        DispatcherTimer dispatcherTimer = new DispatcherTimer();
        dispatcherTimer.Tick += new EventHandler(dispatcherTimer_Tick);
        dispatcherTimer.Interval = TimeSpan.FromMilliseconds(100);
        dispatcherTimer.Start();
    }

    void dispatcherTimer_Tick(object sender, EventArgs e)
    {
        label1.Content = direction + " " + count;

        string directionStr = "east";
        switch (direction)
        {
            case 0: directionStr = "north"; break;
            case 1: directionStr = "east"; break;
            case 2: directionStr = "south"; break;
            case 3: directionStr = "west"; break;
            default: directionStr = "east"; break;
        }
        if (storyboard != null && storyboard.GetCurrentTime() == TimeSpan.FromSeconds(1))//(isWalking == false)
        {
            int[] table = { 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 2, 3, 2 };
            if (direction == 0)
            {
                count = 1;
                spirit.Source = new BitmapImage(
                        new Uri(@"Assets/LinkG_stand_" + directionStr + "00" + table[count] + ".png", UriKind.Relative));
            }
            else
            {
                count = (count > (table.Length - 1)) ? 1 : count;
                spirit.Source = new BitmapImage(
                        new Uri(@"Assets/LinkG_stand_" + directionStr + "00" + table[count] + ".png", UriKind.Relative));
                count++;
            }
        }
        else
        {
            if (count > 10 || count <= 0)
            {
                count = 1;
            }
            else if (count > 0 && count < 10)
            {
                spirit.Source = new BitmapImage(
                    new Uri(@"Assets/LinkG_walk_" + directionStr + "00" + count + ".png", UriKind.Relative));
            }
            else if (count == 10)
            {
                spirit.Source = new BitmapImage(
                    new Uri(@"Assets/LinkG_walk_" + directionStr + "0" + count + ".png", UriKind.Relative));
                count = 1;
            }
            count++;
        }
    }

    /// N - 0
    /// E - 1
    /// S - 2
    /// W - 3
    private int CalculateDirection(Point pTo, Point pFrom)
    {
        pTo.X -= relativePos.X;
        pTo.Y -= relativePos.Y;
        double tan = (pTo.Y - pFrom.Y) / (pTo.X - pFrom.X);
        double delta = pTo.X - pFrom.X;
        if (delta > 0)
        {
            if (tan <= 1 && tan >= -1)
                return 1;
            else if (tan > 1)
                return 2;
            else// if (tan < -1)
                return 0;
        }
        else
        {
            if (tan <= 1 && tan >= -1)
                return 3;
            else if (tan > 1)
                return 0;
            else// if (tan < -1)
                return 2;
        }
    }

    private void Carrier_MouseLeftButtonDown(object sender, MouseButtonEventArgs e)
    {
        isWalking = true;

        Point pFrom = new Point(Canvas.GetLeft(spirit), Canvas.GetTop(spirit));
        Point pTo = e.GetPosition(Carrier);
        direction = CalculateDirection(pTo, pFrom);
        pTo.X -= relativePos.X;
        pTo.Y -= relativePos.Y;


        storyboard = new Storyboard();

        DoubleAnimation doubleAnimation = new DoubleAnimation()
        {
            From = pFrom.X,
            To = pTo.X,
            Duration = TimeSpan.FromSeconds(1)
        };
        Storyboard.SetTarget(doubleAnimation, spirit);
        Storyboard.SetTargetProperty(doubleAnimation, new PropertyPath("(Canvas.Left)"));
        storyboard.Children.Add(doubleAnimation);

        doubleAnimation = new DoubleAnimation()
        {
            From = pFrom.Y,
            To = pTo.Y,
            Duration = TimeSpan.FromSeconds(1)
        };
        Storyboard.SetTarget(doubleAnimation, spirit);
        Storyboard.SetTargetProperty(doubleAnimation, new PropertyPath("(Canvas.Top)"));
        storyboard.Children.Add(doubleAnimation);

        if (!Resources.Contains("rectAnimation"))
        {
            Resources.Add("rectAnimation", storyboard);
        }

        storyboard.Begin();
        storyboard.Completed += new EventHandler((object ssender, EventArgs ee) =>
        {
            isWalking = false;
        });
    }

}
```



就这样，结尾了！萌点在于林克~  虽然最近很忙，不过写一个AVG游戏的念头又冒出来了。。




以上
