---
comments: true
date: 2009-07-03 19:50:05
layout: post
slug: xna-tutorial-3d-ch4-render-3d-titan
title: XNA教程-3D游戏-04-绘制3D地表
wordpress_id: 74
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---




安装好环境、建立了XNA工程、添加了要用到的模型以及贴图，那么接下来要做的就是实际的代码了，而一上来就和2D大为不同的是，我们要直接创建我们的"宇宙环境"。要知道，在2D游戏中，我们游戏的"世界"是整个一张图，就是背景，简单的2D游戏就是这样。而3D则不然，即便是这个环境，那么也是需要我们用模型自己创建的，而这将会是一个非常复杂的工作，究竟如何复杂，看到下面的所有内容你就明白了。




从2D到3D，应该说当前我们在做的是一个非常困难的转换时期，我们要完成第一个3D的绘制，那么就必须了解很多很多只有在3D游戏中才会出现的概念。但是，简单的说，在代码上我们要完成的工作还是类似的，比如2D中，我们先声明了sprite；然后loadContent，或者load确定它的位置等信息；最后我们draw把它绘制出来。那么一样的，3D也是声明、初始化、绘制三个工作。只是2D中无论是初始化还是绘制都是相对简单的，3D就要复杂的多的多，跟着步骤走吧。




* * *







**第一步、创建模型，确定视角**




似乎看了标题就有些茫然，有很多的概念要讲，不过和2D教程一样，先直接说代码怎么写，回头再讲大道理。（本文后面将会有官方解释的翻译内容）




**1、声明场景模型**




代码找到Game1类中最前面，SpriteBatch spriteBatch;这一行的下方，在这里我们先插入这样两行代码。



    
    Model terrainModel;
    Vector3 terrainPosition = Vector3.Zero;
    




这里用到了XNA Framework提供的Model类，以及Vector类。




显然，terrainModel这样的命名让人比较容易理解，就是场景模型，这是我们的场景或者说是地面模型，都一样，就是我们的大环境。




而terrainPostision就更加容易理解了，是terrain的位置，究竟整个地表是怎样的一个位置。这里我们用Vector3这个类来描述。Vector3是一个3维的向量，就像Vector2是X,Y两个轴的向量一样，Vector3实质上就是把Vector2扩展到X,Y,Z，3个轴的向量。初始化为Vector3.Zero，即在整个3维坐标系的原点上。




和2D游戏不同，我们2D游戏所有涉及到的坐标都是在屏幕范围内的。我们知道屏幕向右是X轴正方向，向下是Y轴正方向，所以所有的坐标都是在这样一个坐标系上想象的。而3D游戏则不同，我们必须把这些位置都想象在自己的脑海中，因为屏幕看见的什么只是你3D世界的一角，你必须在整个思维中存在你的自己的一个三维世界，它所在的一个三维坐标系和它的所有坐标。然后我们再讨论从什么视角（什么坐标）去看这个世界。




**2、声明镜头**




说明一下，这里的镜头英文应当是camera，翻译成相机这样子比较好。不过也许很多像我一样的游戏玩家会知道，一般我们评价一个游戏，会说镜头能控制不能控制等等，所以我想这个词会更加容易被接受。我的文章里的镜头，指的就是camera。可以想象你的眼睛就是你看这个现实世界的camera。




在刚才两行代码下面，继续写以下内容。



    
    Vector3 cameraPosition = new Vector3(0.0f, 60.0f, 160.0f);
    Vector3 cameraLookAt= new Vector3(0.0f, 50.0f, 0.0f);
    Matrix cameraProjectionMatrix;
    Matrix cameraViewMatrix;
    




详细说一下我们做了什么。





	
  * cameraPosition：这是镜头所在的位置，比如我们的眼睛在脸上这样的描述。这里是(0, 60, 160)这样一个位置。

	
  * cameraLookAt：镜头的焦点。可以想象一下terrain被放在Y轴也就是高度轴的0上，镜头被放在Z轴也就是深度轴的160的位置，高度是60的位置，而它看着Y轴上50的那点，连成的直线就是你的视线了。

	
  * cameraProjectionMatrix：镜头投影矩阵。

	
  * cameraViewMatrix：镜头视角矩阵。关于两个矩阵，先不详细讲了。图形学中涉及到了任意点的变换可以使用矩阵，矩阵变换是一个图形学非常重要的概念。稍微讲一下。




矩阵变换一般情况下有平移、缩放、旋转。各种复杂的变化都可以由很多这样的操作完成。其他还有诸如投影、切割等等。所有的变化都可以最终表示成为一个矩阵，这就是变换矩阵。不详细讲，举3个例子。




**例一**：平移 W(wx,wy,wz)




![](/upload/2009-07-03_Matrix1.jpg)




**例二**：缩放 T(tx,ty,tz)




![](/upload/2009-07-03_Matrix2.jpg)




**例三**：绕X轴旋转alpha度




![](/upload/2009-07-03_Matrix3.jpg)




当然在XNA framework里封装的Matrix类不是一个简单的矩阵的实现了，还加上了很多智能的功能，当然也包括变换，至于如何使用这样一个Matrix，以及我们声明的Matrix是做什么样的变换的，看下去。




**3、初始化变换矩阵**




我们要初始化声明的资源，那么就要找到LoadContent()方法，在其中TODO的位置加上这样两句。



    
    cameraViewMatrix = Matrix.CreateLookAt(
        cameraPosition,
        cameraLookAt,
        Vector3.Up);
    cameraProjectionMatrix = Matrix.CreatePerspectiveFieldOfView(
        MathHelper.ToRadians(45.0f),
        graphics.GraphicsDevice.Viewport.AspectRatio,
        1.0f,
        10000.0f);
    




这里做的工作是实例化两个Matrix，但是这里用到了Matrix类中的CreateLookAt和CreatePerspectiveFieldOfView方法。说下参数的意思。





	
  * CreateLookAt：
		
			
    * cameraPosition：Vector3，镜头所在位置

			
    * cameraLookAt：Vector3，镜头焦点坐标

			
    * Vector3.Up：Vector3，按照MSDN所说传入"上"就好，当然你可以试一下其他方向……

		
	

	
  * CreatePerspectiveFieldView：
		
			
    * MathHelper.ToRadians(45.0f)：float，视角大小

			
    * graphics.GraphicsDevice.Viewport.AspectRatio：float，窗口的比率，"宽"比上"长"的比率

			
    * 1.0f：float，镜头最小深度（近处）

			
    * 10000.0f：float，镜头最大深度（远处）（恐怕真三国无双和GTA这样的游戏里面这个参数的体会会比较深刻）

		
	




初始化完毕，就可以接下去继续了。




* * *







**第二步、编写模型绘制方法**




因为游戏中将会出现很多很多的模型，虽然每一个模型都有其特殊的地方，但是每一个模型都将拥有一个默认的绘制方式。而，即便采用默认的方式，绘制模型也将会涉及到很多操作，所以将本来可以在draw方法中完成的内容单独的写一个方法来完成。方法传入的参数为模型和其所在的位置，这样我们的方法就可以把它用一样的操作绘制出来。




这里涉及到一个ModelMesh的类，每一个Model都一个很多的ModelMesh的集合，因此我们需要单独绘制每一个mesh。至于ModelMesh和Model的概念，在后面会加上说明。（本文后面将会有官方解释的翻译内容）




具体方法是，在Game1类中重新写一个方法，当然是任意位置的，代码具体如下。



    
    void DrawModel(Model model, Vector3 modelPosition)
    {
        foreach (ModelMesh mesh in model.Meshes)
        {
            foreach (BasicEffect effect in mesh.Effects)
            {
                effect.EnableDefaultLighting();
                effect.PreferPerPixelLighting = true;
                effect.World = Matrix.CreateTranslation(modelPosition);
                effect.Projection = cameraProjectionMatrix;
                effect.View = cameraViewMatrix;
            }
            mesh.Draw();
        }
    }
    




如参正如之前所说，一个是Model类，一个是Vector3类，分别表示要绘制的模型和其所在的位置。





	
  * 第一个foreach循环是遍历Model中所有的ModelMesh，单独处理每一个。

	
  * 第二个foreach循环是遍历ModelMesh中的Effects，单独处理每一个效果的处理。具体看看做了一些什么
		
			
    * EnableDefaultLighting()：设定光源为默认光源。

			
    * effect.PreferPerPixelLighting = true;：在支持Pixel Shader Model 2.0的情况下，开启每个像素按光源渲染的效果。

			
    * effect.World = Matrix.CreateTranslation(modelPosition);：创建传入模型的矩阵变换

			
    * effect.Projection = cameraProjectionMatrix;：设定投影变换矩阵。

			
    * effect.View = cameraViewMatrix;：设定视角变换矩阵。

		
	




这样，我们就完成了绘制方法的编写。但是不要忘了，我们还没有加载需要绘制的模型，也没有在draw方法中调用这个方法，所以看接下去的两步，完成这两个工作。




* * *







**第三步、加载模型**




载入模型才能画，载入模型的方法和2D的一模一样，只是一个操作的是Sprite，一个操作的是Model，但是我们知道LoadContent方法是支持范型的，那么显然我们的工作就变得非常的简单了。




找到LoadContent方法，在之前添加的代码下面加上如下代码。



    
    terrainModel = Content.Load<Model>(@"Modelsterrain");
    




这样，这一步就简简单单的完成了。




* * *







**第四步、绘制模型**




之前第二步完成的工作就大大简化了这一步我们的代码了。而这一步我们就只要调用之前我们写的drawModel方法，并且传入我们的模型和位置就可以了。




在Draw方法的TODO处，加入以下代码：



    
    DrawModel(terrainModel, terrainPosition);




还等什么呢？现在就是编译运行的时候了！效果图。




[![](/upload/2009-07-03_Runtime.jpg)](/upload/2009-07-03_Runtime.jpg)




下面加了4张修改了参数之后的图，方便理解一些参数。[点击放大]




[![](/upload/2009-07-03_Runtime_Down.jpg)](/upload/2009-07-03_Runtime_Down.jpg) [![](/upload/2009-07-03_Runtime_100CameraHeight.jpg)](/upload/2009-07-03_Runtime_100CameraHeight.jpg) [![](/upload/2009-07-03_Runtime_95Radians.jpg)](/upload/2009-07-03_Runtime_95Radians.jpg) [![](/upload/2009-07-03_Runtime_5000Depth.jpg)](/upload/2009-07-03_Runtime_5000Depth.jpg)





	
  * cameraViewMatrix实例化，CreateLookAt时，修改第三个参数为Vector3.Down

	
  * cameraPosition声明并实例化时，修改为Vector3(0.0f, 100.0f, 160.0f)

	
  * cameraProjectionMatrix实例化，CreatePerspectiveFieldOfView时，修改第一个参数为90.0f

	
  * cameraProjectionMatrix实例化，CreatePerspectiveFieldOfView时，修改第四个参数为5000.0f




【官方源代码下载】




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=159)




* * *







那么下面就贴翻译的内容，也算是比较重要的东西了，2D和3D游戏的概念上的区别。




（以下翻译为本人原传，未经允许严禁转载。另外，因本人能力有限难免出错，请高手指正。原址点击[这里](http://creators.xna.com/en-US/education/gettingstarted/bg3d/details/4_1)。）




**1、3D世界：2D游戏和3D游戏之间的区别**




> 
	
> 
> 如果你学习过第一个教程（创建2D游戏），那么你已经变得习惯用2D空间来思维了。位置、方向和动作可以全都用Vector2和一个单独的旋转角度来表示。
> 
> 
	
> 
> 这个教程介绍了在三维的概念，和其一系列的新的对象。从根本上，3D世界区别于2D世界的就是新增的第三个坐标轴----Z轴，或者说是深度。在XNA Game Studio中，2D世界实质上是一个特殊的，受限制的3D世界的版本----你将会在本教程读到的计算也同样在2D中可以读到。
> 
> 
	
> 
> 在XNA Game Studio这哦你，3D的变化意味着，取代在平面世界中画静态的Texture2D对象的是绘制用Model类表示的3D模型。Model类是被叫做网络（mesh）的许多有联系的3D点的集合，在整个世界中，最终渲染在屏幕上的对象，取决于对象的位置和方向，和镜头的位置和方向。
> 
> 
	
> 
> [![](/upload/BG_4.4.1.1pd.png)](/upload/BG_4.4.1.1pd.png)这过程也许初看十分复杂，但其实在3D世界编程，只依靠一些非常基本的元素。想象一个点在3D空间中，一个点有一个X坐标,一个Y坐标,和一个Z坐标。非常类似2D空间中的一个点，3D空间中的一个点也可以使用一个向量来移动，但是在3D空间中，我们使用包含Z坐标的Vector3类。
> 
> 
	
> 
> 然而，使用一个叫做矩阵的数学结构体，可以使3D点做更多的事情。这些结构体，在XNA Framework中表示为Matrix类，它描述了一系列可以应用于点的变换、点的集合、或一个响亮。使用矩阵，3D对象可以移动、旋转、切割、以及（通过使用被叫做视点矩阵和投影矩阵的特殊的矩阵）绘制成一个可以显示在屏幕上的2D图像。
> 
> 
	
> 
> 想象有一系列我们想画在屏幕上的3D世界中的点。它们拥有相对于被叫做原点的中心点的位置和方向信息。如果没有任何的变换，这些点只是存在于他们自己的空间，被叫做"对象空间"。
> 
> 
	
> 
> 当你加载一系列的点，并且把它们添加到你的3D世界中，你可能会想给他们一个在3D世界中的位置，用来旋转或切割他们。这些行动的产物被叫做世界变换矩阵。应用了这样的矩阵的点，就被想象成在"世界空间"中（而不是对象空间中）了。
> 
> 
	
> 
> 来渲染在用户屏幕上的点，需要一些附加的信息。没有要绘制的2D场景的角度和位置，是不可能把整个3D常见转换到2D图像的。使用一个视点是必要的，也被称作镜头，只需绘制它所看到的3D场景。在准备这次绘制时，镜头信息会被计算到一个视角矩阵中，把3D点的方向和位置都和镜头联系起来。之后，这些3D点就被想象成在"视觉空间"中了。
> 
> 
	
> 
> 用2D绘制一个3D场景最后还需要一些其他信息。像视点的视角和领域的信息，控制着3D点如何绘制在2D计算机屏幕上。这些信息存储在一个投影矩阵中，应用之后，把3D坐标的系统中的点转换到2D坐标系统中，2D坐标系统被成为"屏幕空间"，这样点被绘制成像素。
> 
> 
	
> 
> [![](/upload/BG_4.4.1.2pd.png)](/upload/BG_4.4.1.2pd.png)
> 
> 
	
> 
> 如果要套用这个概念到3D点的集合所组成的网络（mesh）上（可以是3D表示的任何东西，一个飞船、一个人、或一只恐龙），你只需用到一些在XNA Framework中的基本的对象来制作你的游戏----Mesh类，Matrix类和Vector类。
> 
> 








** 2、创建一个模型绘制方法**




> 
	
> 
> 绘制一个3D模型到屏幕上是一个复杂的工作。它包括了庞大数据量的多边形的绘制，而它们经常被构造成许多不同的方式。有些可能是基于艺术效果的，而有些也许是倾向于程序化的。绘制同时也是必要的工作。每个游戏中可见的3D对象都是需要经过一个绘制过程的。
> 
> 
	
> 
> 毫无悬念的，不是只有一种组织一个模型绘制方法的方式。参数、界限以及绘制算法对每个游戏来说都是不同的，取决于游戏的需求和涉及的技术。在这样的情况下，XNA Framewrok没有尝试去提供一个单独的模型绘制方法。取而代之的是提供一些元素，而把组织它们的工作留给了程序员来做。
> 
> 
	
> 
> 一个Model类是XNA Framework中表示3D模型的一个基本对象，但是它比最初的模型要复杂许多。一个模型由一个或多个ModelMesh对象以一个集合的形式组成。每个ModelMesh对象是一系列组成网络（mesh）的顶点的集合，它们可以在同一个模型中被单独的移动。
> 
> 
	
> 
> 一个坦克的模型是一个很好的例子。坦克的身体也许是一个ModelMesh，坦克的炮台也许是另外一个。虽然你绘制的是整个坦克，但是你可能需要分开移动坦克的不同部分。你可以使用ModelMesh对象的集合来移动不同的部分。
> 
> 
	
> 
> ModelMesh包含了一个绘制方法。每绘制一个ModelMesh到屏幕上需要调用这个方法一次。然而如果你没有实现设定变换和MedelMesh上的光的属性，那么你什么也看不到。
> 
> 
	
> 
> 变换（决定了ModelMesh在世界中的定向，并且最终，物体是否出现或是出现在屏幕的哪里）和光（）没有被应用到ModelMesh层面。相对的，每个ModelMesh有一些Effect和它联系在一起。一个Effect对象可以想成提供给可视设备的说明，关于如何绘制一个ModelMesh。
> 
> 
	
> 
> 在很多游戏中，Effect对象是由很多复杂的顶点和像素遮罩（shader）组成的。在XNA Game Studio中也可以实现，并且对于高级图形效果来说十分重要。但是在简单的渲染中，XNA Framework Content Pipline（内容管道）提供了一个简单的机制来完成基本的光和绘制效果 ---- BasicEffect类。
> 
> 
	
> 
> 每个通过content pipline加载的ModelMesh接受了一个或多个BasicEffect对象，它们可以在不处理像素或顶点遮罩（shader）的情况下简单的处理变换和光源效果。所有这些BasicEffect类需要一系列表示世界（World）矩阵，视点（View）矩阵和投影（Projection）矩阵的Matrix类，和一些光照的参数。可以调用BasicEffect.EnableDefaultLighting()方法获得默认的参数。
> 
> 
	
> 
> 在BasicEffect的参数被设置后，剩下的就只有调用ModelMesh.Draw方法，使用之前为BasicEffect设置的参数，来绘制ModelMesh到屏幕上了。
> 
> 
	
> 
> [![](/upload/BG_4.4.2.1pd.png)](/upload/BG_4.4.2.1pd.png)
> 
> 

