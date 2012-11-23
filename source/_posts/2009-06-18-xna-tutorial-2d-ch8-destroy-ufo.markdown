---
comments: true
date: 2009-06-18 22:13:16
layout: post
slug: xna-tutorial-2d-ch8-destroy-ufo
title: XNA教程-2D游戏-08-摧毁UFO
wordpress_id: 68
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---




**XNA教程 -- 2D游戏 -- 08 -- 摧毁UFO**




这一次要做的是很NB的事情，碰撞检测（Collision Detection），具体到我们的例子，具象一点的，那么就是摧毁UFO，当然在无数游戏中，碰撞检测都是至关重要的东西，和随机比起来不相上下吧。无论是什么游戏类型，物体和物体之间的接触都是要处理的，尚且不说加上什么物理特性，光是这个例子里的子弹碰UFO总要有点反应吧。更不用说什么火星撞地球了………………




具体代码很简单，找到UpdateCannonBalls方法，在更新的时候，添加一种杀掉UFO和炮弹的情况，就是两者碰撞的时候。




大致的思路为：





	
  1. 确定每个炮弹的范围大小和UFO的范围大小。

	
  2. 遍历每个炮弹。

	
  3. 遍历中每个炮弹再要遍历每个UFO。如果发生碰撞就同时摧毁两个（alive = false）




这里我们要用到的碰撞检测，直接使用XNA的Rectangle的intersect方法。以下是代码，添加在判断是否出屏幕的下面。



    
    Rectangle cannonBallRect = new Rectangle(
        (int)ball.position.X,
        (int)ball.position.Y,
        ball.sprite.Width,
        ball.sprite.Height);
    foreach(GameObject enemy in enemies)
    {
        Rectangle enemyRect = new Rectangle(
            (int)enemy.position.X,
            (int)enemy.position.Y,
        enemy.sprite.Width,
        enemy.sprite.Height);
        if(cannonBallRect.Intersects(enemyRect))
        {
            ball.alive = false;
            enemy.alive = false;
            break;
        }
    }
    




首先获得当前遍历到的cannonBall的范围，循环。循环中得到当前UFO的范围，检测是否和cannonBall相交（哪一方都一样）。碰撞了，就两个对象的alive都为false，然后break出UFO的循环。




执行结果（其实截图看不出来效果唉）




[![](/upload/2009-06-18_Chapter8Runtime.jpg)](/upload/2009-06-18_Chapter8Runtime.jpg)




【官方源代码下载】




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=155)




* * *







**翻译：使用Rectangle的2D碰撞检测**




（以下文字由本博客翻译自这里，转载请注明出处）




> 
	
> 
> 图片创建的Sprite从一创建就必然是一个长方形。你可以使用遮盖(Mask)，来绘制Sprite中的一部分像素，这样就产生了Sprite不是一个长方形的视觉效果。Sprite可能呈现为一个圆形的物体，或者更加复杂的形状。但是所有的sprite一创建就都是矩形的。
> 
> 
	
> 
> Sprite的这一特性在非常基本的碰撞检测中很实用，XNA Framework提供了一个Rectangle类，自己就拥有碰撞帮助的方法：ectangle.Intersects，和Rectangle.Contains。
> 
> 
	
> 
> 在基本的常识中，碰撞检测是基于相交检测的 ---- 两块几何图形是否相交。如果两个几何图形相交，他们可以说是碰撞，游戏逻辑可以根据碰撞的物体产生适当的反应。
> 
> 
	
> 
> 在我们的2D游戏中，我们关心加农炮是不是和地方UFO相交。如果加农炮的sprite的rectangle相交于敌人sprite的rectangle，那我们的游戏逻辑检测到了一个碰撞，并且同时摧毁两个物体。
> 
> 
	
> 
> Rectangle很容易创建。Rectangle类初始化时需要4个参数 ---- 矩形左上角的点的X和Y坐标，以及矩形的宽度和高度。所有矩形的其他点都会有这些信息计算出来。
> 
> 
	
> 
> 创建sprite占用的rectangle之间的碰撞，设置X和Y的值为游戏世界中的sprite的X和Y值，GameObject.position.X和GameObject.position.Y。设置宽度和高度为sprite中的Texture2D的宽度和高度，GameObject.sprite.Width和GameObject.sprite.Height。
> 
> 
	
> 
> 凭借两种游戏直接中sprite的rectangle对象的创建和调用Rectangle.Intersects方法，你可以知道两个物体是否碰撞。这可能不是最精确的碰撞。包括还有其他的缺点，如果是屏幕上已经画了的一个旋转过的sprite，rectangle对象的定义就不再符合sprite在屏幕上绘制的样子了。然而这是一个简单高效的方法来做碰撞检测。
> 
> 
	
> 
> 一个类似的有用的方法是Rectangle.Contains。在我们的2D游戏中，使用一个rectangle来表示整个屏幕，我们可以定义什么时候GameObject离开了屏幕的边界。我们在比较屏幕rectangle和对象rectangle的时候，使用检查Rectangle.Contains的值是否为false的方法来判断是否离开屏幕。
> 
> 
	
> 
> 下面的图表列出了Rectangle.Intersects和Rectangle.Contains两个不同的矩形成员的不同输出情况。
> 
> 
	
> 
> ![](/upload/BG_3.8.1.1pd_a.png)
> 
> 

