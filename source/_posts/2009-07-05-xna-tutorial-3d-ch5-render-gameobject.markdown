---
comments: true
date: 2009-07-05 11:48:03
layout: post
slug: xna-tutorial-3d-ch5-render-gameobject
title: XNA教程-3D游戏-05-绘制GameObject
wordpress_id: 75
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---




从2D的意识转换到3D游戏的意识之后（也许还有直接接触3D的），之后的工作就和2D一样简单了，想想教程的例子要有什么——加农炮、UFO、炮弹，就是全部了。我们要一个一个的添加到游戏中，但是和2D一样，我们一定会有大量重复的工作，所以我们需要把这些代码封装，所以我们将创建一个具有一般性的类GameObject，来代替所有的加农炮、UFO等等物件的凌乱的代码。




**第一步、创建GameObject类**




GameObject类究竟有什么好处，烦请大家看下[这里](http://arthraim.cn/view.asp?id=65)的末尾《为什么要创建自己的类》这一部分，当然，有过任何OO语言基础的人也知道为什么要封装一个类。




写这个类和2D的如出一辙，一样的我们需要有他的模型Model（sprite）、位置Position、旋转rotation。再加上一个scale属性。至于scale是哪一种，我们这里只是用来缩放模型大小的最最简单的。




> 
	
> 
> 右键单击工程 -> Add -> New Class -> 命名为GameObject.cs
> 
> 





然后修改类为如下代码：



    
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using Microsoft.Xna.Framework;
    using Microsoft.Xna.Framework.Audio;
    using Microsoft.Xna.Framework.Content;
    using Microsoft.Xna.Framework.GamerServices;
    using Microsoft.Xna.Framework.Graphics;
    using Microsoft.Xna.Framework.Input;
    using Microsoft.Xna.Framework.Media;
    using Microsoft.Xna.Framework.Net;
    using Microsoft.Xna.Framework.Storage;
    namespace YourNamespace // 注意修改
    {
        class GameObject
        {
            public Model model = null;
            public Vector3 position = Vector3.Zero;
            public Vector3 rotation = Vector3.Zero;
            public float scale = 1.0f;
        }
    }

这样一个通用的GameObject类就简简单单写完了。





	
  * Model为null，防止没有被实例化的情况。

	
  * 两个Vector3都为Zero，位置在原点，没有方向。

	
  * scale为1.0f，表示1.0倍的大小。




* * *







**第二步、应用GameObject到terrain**




既然有了GameObject类，我们就应该把它应用到所有的要加入我们的3D世界的模型中，那么terrain也不例外。所以这一步我们稍稍花点时间，把之前写的terrain的代码修改一下，应用GameObject类。之前有3处地方要写，那么现在就有3处地方要改，声明、初始化、绘制。因为绘制还要做其他的修改，所以我们这里先修改声明和初始化部分。




（注释为原先的代码，其他为新编写的代码）




**1、声明。**Game1类刚刚开始的地方，声明很多变量的地方。



    
    //Model terrainModel;
    //Vector3 terrainPosition = Vector3.Zero;
    GameObject terrain = new GameObject();

**2、初始化 。**找到LoadContent()中和terrain有关的部分。



    
    //terrainModel = Content.Load<Model>(@"Modelsterrain");
    terrain.model = Content.Load<Model>(@"Modelsterrain");

这样，terrain就成为了第一个GameObject的应用。接下去。




* * *







**第三步、创建新GameObject，MissleLauncherBase**




MissleLauncherBase就是指加农炮的基座，基座是不动的，放在一个固定的位置，我们还是要使用GameObject类来创建他，和terrain是一样的。




**1、声明**。Game1类刚刚开始的地方，声明很多变量的地方。



    
    GameObject missleLauncherBase = new GameObject();

**2、初始化 。**找到LoadContent()中和terrain有关的部分。



    
    missleLauncherBase.model = Content.Load<Model>(@"Modelslauncher_base");
    missileLauncherBase.scale = 0.2f;

加上scale的属性，确定缩放。一样把绘制部分交给最后一步统一完成。




* * *







**第四步、绘制GameObject**




之前我们绘制过terrain，但是我们使用的是terrainModel这个Model类，和terrainPosition这个Vector3类。现在我们已经将GameObject引入了我们的游戏中，所以我们要重新修改DrawModel方法，我们不需要一个模型一个模型的画，只需要把GameObject这样一个通用的对象，采用一种通用的方法画就可以了。




所以找到DrawModel方法，我们重新写一个DrawGameObject方法（DrawModel方法当然一定用不着了，删掉也行），入参为GameObject。很好，代码如下。



    
    void DrawGameObject(GameObject gameobject)
    {
        foreach (ModelMesh mesh in gameobject.model.Meshes)
        {
             foreach (BasicEffect effect in mesh.Effects)
             {
                 effect.EnableDefaultLighting();
                 effect.PreferPerPixelLighting = true;
                 effect.World =
                     Matrix.CreateFromYawPitchRoll(
                         gameobject.rotation.Y,
                         gameobject.rotation.X,
                         gameobject.rotation.Z) *
                     Matrix.CreateScale(gameobject.scale) *
                     Matrix.CreateTranslation(gameobject.position);
                 effect.Projection = cameraProjectionMatrix;
                 effect.View = cameraViewMatrix;
             }
             mesh.Draw();
        }
    }

不难发现我们对effect.World的初始化发生了一些变化，原本我们只是使用了Matrix.CreateTranslation方法，而现在还添加了两个方法，并且取了三个方法的乘积。这其实就像我[上一章](http://arthraim.cn/post/2009/07/74.html)提到过的所有的变化都可以转换为多个变换矩阵的乘积。于是在代码里我们把表示旋转、缩放、位移的三个矩阵相乘，来完成所有这样三个的变化（下面资料中将详细讲解，这三个矩阵被翻译成旋转、比例、转换，其实旋转、缩放、位移更加易于理解）。另外，下一章中，加入加农炮的发射塔，这些参数将变得可控（虽然当前的terrain和missleLauncherBase都是不变的），所以这里依然使用了这样的方式，为了这个方法的通用性。（之后关于此内容还将有官方说明内容）




完成对这个方法的修改，只需要修改Draw方法中的代码就可以了。



    
    DrawGameObject(terrain);
    DrawGameObject(missileLauncherBase);

就这样取代之前的DrawModel的调用，加上两行，每一个GameObject只需要一行的调用。这样绘制工作就算完成了。




运行一下，看看实际效果吧！




[![](/upload/2009-07-05_Runtime.jpg)](/upload/2009-07-05_Runtime.jpg)




【官方源代码下载】




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=160)




* * *







**特别说明**：这段翻译，其实讲的就是世界变换的3个矩阵的情况，因为rotation scale translation这三个词中文的叫法是什么我不太清楚，所以这里权且翻译成旋转、比例、转换这样子，尤其是转换这个词不太好，不过因为translation真正完成的工作其实就是平移矩阵，相当于坐标轴的转换，所以只取了转换两个字，希望不会对大家理解上产生困扰。




（以下文字翻译自这里，未经允许禁止转载。由于本人水平有限，所以有任何错误希望高手更正。）




**翻译：构造世界的变换**




> 
	
> 
> 3D模型由3D建模软件创建。在这些软件包中，美工们创建了3D世界中的模型。这个世界有一个中心，叫做原点，并且模型根据它来创建。位置、旋转和比例都和相对原点相关联，并且当游戏载入一个模型，会保留他的位置、旋转和比例关系。这样，这个模型被称作在“对象空间”或“模型空间”中。
> 
> 
	
> 
> 当一个游戏绘制一个模型时，它连接了模型空间中的原点和游戏世界中的原点。如果这个模型在3D建模软件中存储的中心位置正好是模型空间的原点，那在游戏中绘制的时候他就会出现在游戏世界的原点。
> 
> 
	
> 
> 当在你的游戏中绘制一个3D模型，你需要把3个变换应用到你的3D模型上。三者中的第一个，“世界转换”，定义如何转换你的模型的初始位置、方向和比例（对象空间）到游戏世界中的某个位置、方向和比例。
> 
> 
	
> 
> 一个标准的世界转换分别由三个矩阵组成（旋转、比例和转换），所有的都将被整合成一个世界变换矩阵。
> 
> 
	
> 
> 
		
>   * 
			
> 
> -- “旋转”（rotatiton）变化模型角度的方向——你可以把它想象成在空间中根据一点朝不同的方向旋转模型。有很多计算和存储旋转数值的方法，一种最常用的就是欧拉角，围绕三个轴旋转一个对象，X轴、Y轴和Z轴。（下一节会解释和说明欧拉角）。创建一个旋转矩阵，你可以使用Matrix.CreateFromYawPitchRoll方法，传入围绕各个轴的旋转角度。要存储游戏对象的这些角度，Vector3类是一个很好的选择。
> 
> 
		
> 
		
>   * 
			
> 
> -- “比例”（scale）变化模型的尺寸，使它能更大或更小。因为它可以让模型沿着任意轴拉伸，它更多的被应用到统一的比例，使得整个模型缩小或放大。要创建一个比例矩阵，使用Matrix.CreateScale方法，传入你要缩小或放大模型的因数。你可以存储游戏对象的比例因数为一个简单的float类型。
> 
> 
		
> 
		
>   * 
			
> 
> -- “转换”（translatiton）沿着3D世界的3个轴移动整个模型。想象成到处平移模型：左右移动（X轴），上下移动（Y轴），前后移动（Z轴）。创建一个转换矩阵，使用Matrix.CreateTranslation方法，传入你要沿着各个轴移动的数据。存储转换的数值可以使用Vector3。
> 
> 
		
> 
	
	
> 
> 一旦你创建了这些矩阵，你必须把它们组合成为一个单独的世界变换矩阵。这个工作被称为级联（concatenation）。在XNA Game Studio中，级联一个矩阵和另一个矩阵，使用乘法符号（*）。你必须使用正确的顺序级联这些矩阵。
> 
> 
	
> 
> _世界变换 = 旋转 × 比例 × 转换。_  

		_World Transformation = Rotation * Scale * Translation_
> 
> 
	
> 
> 级联的顺序十分重要。因为矩阵惩罚是没有交换律的，所以用错误的顺序相乘会产生预期之外的结果——比如意外的在旋转之前转换坐标系，相对世界原点旋转了坐标系转换后的模型，使模型到了比起简单的正常旋转后的世界空间中的位置非常非常遥远的其他位置，
> 
> 
	
> 
> 修改几次每个独立的变换矩阵传入的数值，你可以是物体在世界中穿过（转换），在空间中旋转（旋转），平滑的缩小或放大，或者三个的组合效果。
> 
> 

