---
comments: true
date: 2009-06-19 00:03:20
layout: post
slug: xna-tutorial-2d-ch9-record-scores
title: XNA教程-2D游戏-09-记录分数
wordpress_id: 69
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---




游戏基本上已经什么都有了，不过接下去还有一些工作 ---- 记分。通过击毁UFO来获得分数，这是一个简单的功能，不过还是什么必要的，因为我们不单单是记住分数，还要把它显示在屏幕上，所以如何在屏幕上显示字符才是关键所在。如果你愿意，可以显示一些其他信息，比如常见fps等等。




那么接下来，介绍方法。




**第一步、添加Font到Content中**




**一、添加SpriteFont的Content资源。**




要在Content pipline中添加SpriteFont（字体）的内容。





	
  1. **Content中添加一个Fonts文件夹**。  

		![](/upload/2009-06-19_AddFontToContent.jpg)  

		  

		  

		

	
  2. **在Fonts中添加一个GameFont对象**。  

		[![](/upload/2009-06-19_AddSpriteFont.jpg)  

		  

		  

		  

		](/upload/2009-06-19_AddSpriteFont.jpg)

	
  3. **修改XML文件**。具体参照如下代码，字体和字体大小的修改。
		
    
    <FontName>Arial</FontName>
    <Size>18</Size>
    


	







好了，这样准备工作就完成了，我们可以在之后的Content的相关操作中使用这个font了。




**二、声明一些新变量**




声明一个int型的分数。




声明一个SpriteFont对象当作字体。




声明一个Vector2来记录分数显示的位置（百分比）。



    
    int score;
    SpriteFont font;
    Vector2 scoreDrawPoint = new Vector2(0.1f, 0.1f);
    




就像之前说的，微软的大大们都有强大逻辑，可以在开始写程序的时候声明出所有的变量。




**三、Content载入字体**




在LoadContent方法中载入我们感概添加和修改的字体。



    
    font = Content.Load<SpriteFont>(@"FontsGameFont");




这样，我们就可以自由的使用了。




* * *







**第二步、添加记分逻辑**




视频中的逻辑是击毁一个UFO加一分，所以在判断碰撞的地方加上score++就好。




我设计的逻辑击毁加2分，漏掉一个（飞出屏幕）扣一分。




当然你可以设计更加复杂的功能，比如距离远速度快的3分等等，总之分数不是关键。




* * *







**第三步、绘制分数**




绘制我们记分的结果，在Draw这个方法的SpriteBatch.End()之前加入绘制字符串的代码。



    
    spriteBatch.DrawString(
        font,
        "Score:" + score.ToString(),
        new Vector2(
            scoreDrawPoint.X * viewportRect.Width,
            scoreDrawPoint.Y * viewportRect.Height),
        Color.White);
    




看一下参数。





	
  1. SpriteFont，要求传入一个SpriteFont，我们这里传入了之前创建的font。

	
  2. string，传入要显示的字符串，这里是得分。

	
  3. Vector2，传入一个显示位置（左上角）的Vector2，这里我们利用之前创建的百分比和屏幕大小计算出一个新的Vector2

	
  4. Color，传入一个字形的颜色。




好了，这样我们的游戏就完整的完成了。放一张运行时的截图。




[![](/upload/2009-06-19_Chapter9_Runtime.jpg)](/upload/2009-06-19_Chapter9_Runtime.jpg)




【官方源代码下载】




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=156)
