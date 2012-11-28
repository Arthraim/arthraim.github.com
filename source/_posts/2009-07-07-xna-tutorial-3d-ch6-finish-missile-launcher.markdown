---
comments: true
date: 2009-07-07 16:20:07
layout: post
slug: xna-tutorial-3d-ch6-finish-missile-launcher
title: XNA教程-3D游戏-06-完成炮台
wordpress_id: 76
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---




之前完成了GameObject类的一些基础工作，也因此，我们可以使得terrain和missle launcher base利用GameObject绘制在了屏幕上。应该记得，我们添加发射塔的时候说明了，这只是一个基座（base），至于炮塔部分其实是没有被添加的，而这就是我们这次要完成的工作。不但要完成这个发射塔，还要使得它可以被用户控制，对用户输入产生反应，那么现在开始吧。




**第一步、绘制炮塔的GameObject**




这一步的当然是非常的简单，我们只是把没有画上去的那一半处理好就可以了，添加一个新的GameObject，步骤还是声明和初始化。




**1、声明。**找到声明部分，在已经添加的两个GameObject下面添加一个新的GameObject的声明，代码如下：




    GameObject missileLauncherHead = new GameObject();




代码很简单。




**2、初始化。**找到LoadContent方法，在初始化另外两个GameObject的代码下面添加以下代码：




    missileLauncherHead.model = Content.Load<Model>(
        @"Modelslauncher_head");
    missileLauncherHead.scale = 0.2f;
    missileLauncherHead.position = missileLauncherBase.position +
        new Vector3(0.0f, 20.0f, 0.0f);




讲解一下：






  1. 首先是载入asset，没有什么问题。


  2. 定义比例为0.2f，即和base一样。


  3. 下面初始化position的位置，实在原有的Base位置上加了(0, 20, 0)这样一个向量，意味着炮塔比基座升高了20。




这里不妨先完成第三步，看看效果。不过按照之前的习惯，都是完成了逻辑代码之后再绘制的。（微软的工程师都是天才，囧）




![](/images/uploads/zb/2009-07-07_151248.jpg)![](/images/uploads/zb/2009-07-07_151225.jpg)




不难看出变化吧～




* * *







**第二步、加入输入控制**




这一步我们要让这个炮台可以被我们控制，方法很简单，和2D的非常的类似，如果你有2D基础那么就非常容易理解这个部分的内容了，如果没有我下面可能还会讲一下，你也可以回顾一下[2D教程](http://arthraim.cn/post/2009/06/65.html)中相似的内容，讲解对应XBOX360手柄GamePad类的一些坐标系的说明。




**1、添加XBOX360的控制代码。**




    GamePadState gamePadState = GamePad.GetState(PlayerIndex.One);
    missileLauncherHead.rotation.Y -=
        gamePadState.ThumbSticks.Left.X * 0.1f;
    missileLauncherHead.rotation.X +=
        gamePadState.ThumbSticks.Left.Y * 0.1f;




GamePadState类用来获取手柄的操作情况，GamePad.GetState方法来决定获取哪个手柄的操作。因为XBOX360可以连接多个最多4个手柄，所以PlayerIndex这个枚举中分别由One,Two,Three,Four四个值，对应1P 2P 3P 4P手柄。




gamePadState.ThumbSticks.Left对应XBOX360手柄的左边的类比摇杆（传统游戏机上的方向键，microsoft用摇杆替代了方向键，顺应欧美玩家的习惯）。另外相对还有一个Right，当然是在右边。X轴决定左右，Y轴决定上下。




那么手柄的左右，决定了围绕Y轴（纵向轴）的旋转；上下决定了围绕X轴（横向轴）的旋转。还有个Z轴的旋转我们没有用到，如果是飞行模拟游戏里，一般左右控制Z轴，而另外L,R键来控制X轴。




**2、添加键盘的控制代码。**




    #if !XBOX
    KeyboardState keyboardState = Keyboard.GetState();
    if(keyboardState.IsKeyDown(Keys.Left))
    {
        missileLauncherHead.rotation.Y += 0.05f;
    }
    if(keyboardState.IsKeyDown(Keys.Right))
    {
        missileLauncherHead.rotation.Y -= 0.05f;
    }
    if(keyboardState.IsKeyDown(Keys.Up))
    {
        missileLauncherHead.rotation.X += 0.05f;
    }
    if(keyboardState.IsKeyDown(Keys.Down))
    {
        missileLauncherHead.rotation.X -= 0.05f;
    }
    #endif




#if和#endif之间表示如果不在XBOX情况下。




其他代码非常的相似，0.05f是个幻数，其实表示了增加量。 这个值是固定的，使得用键盘操作和手柄操作产生了一些差别。




上下左右分别控制两个轴的变化，比手柄更易理解。




**3、限定旋转角度。**




炮塔的旋转不能任意，不太符合逻辑，所以要把它限定一下，使用MathHelper类，一样在2D教程中详细的讲解了这个类，从3D开始看教程的可以看一下[这篇文章](http://arthraim.cn/post/2009/06/65.html)的第二步第3小节和末尾对角度的讲解。




这里要写的代码如下：




    missileLauncherHead.rotation.Y = MathHelper.Clamp(
        missileLauncherHead.rotation.Y,
        -MathHelper.PiOver4,
        MathHelper.PiOver4);
    missileLauncherHead.rotation.X = MathHelper.Clamp(
        missileLauncherHead.rotation.X,
        0,
        MathHelper.PiOver4);






  * 第一次调用限定了rotation的Y轴，也就是左右的范围在-45度到45度之间。


  * 第二次调用限定了rotation的X轴，也就是上下的范围在0度到45度之间。




如果你添加了Draw部分的代码，那么现在已经完成了，没有添加，那就是我们下面要做的工作。




* * *







**第三步、绘制炮台**




正如之前所说，非常简单的第三步，就是在Draw方法中插入一行代码，和之前的terrain和missleLauncherBase类似的代码。如果你是从第一步跳跃过来的，那么也一样，马上会有效果。




在另外两个GameObject的操作的下面插入代码：




    DrawGameObject(missileLauncherHead);




OK，编译、运行～




[![](/images/uploads/zb/2009-07-07_Runtime.jpg)](/images/uploads/zb/2009-07-07_Runtime.jpg)




大功告成，炮台可以旋转，并且在一定的范围之内了，试着变化这个范围DIY一下。




【官方源代码下载】




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=161)




* * *







**特别说明：**这段翻译，讲解了3D物体在游戏空间中的旋转情况，也就是pitch, yaw, roll三种情况。一样的，在翻译这三个单词的时候带来了一些困扰，权衡后决定翻译成您现在看到的样子，尤其是pitch不太贴切。




（以下文字翻译自XNA Creator Club网站，未经允许禁止转载。由于本人水平有限，所以有任何错误希望高手更正。）




**3D空间中的旋转：欧拉旋转**




>

>
> 任意世界变换矩阵的一个重要组成部分就是旋转。旋转对应了一个对象相对于特定的轴的角度方向。
>
>

>
> 最终，一个物体的旋转将使用一个级联了"转换"（translation）矩阵和"比例"（scale）矩阵而约束的世界变化矩阵来决定。
>
>

>
> 我们在这个教程中使用的方法包括了一系列被叫做欧拉旋转的中间旋转。想象欧拉旋转为一系列的3个不同弧度的角，从0到2π，分别在不同的三个轴上。这些轴（X轴,Y轴,Z轴）可以被想象为低头、偏转、翻转轴（pitch, yaw and roll axes）。
>
>

>
> [![](/images/uploads/zb/BG_4.6.2.1pd.png)](/images/uploads/zb/BG_4.6.2.1pd.png)
>
>

>
> 低头（picth），根据X轴测量，可以被想象为飞机头的方向向上或向下。

		偏转（yaw），根据Y轴测量，使得飞机从一边摆向（swing）另一边。

		翻转（roll），根据Z轴测量，可以想象为飞机翻向（banking）左边或右边。

		（英文中中西表述方式不同，难以理解，图片非常说明问题）
>
>

>
> 在XNA Framework中使用欧拉旋转来创建世界变换矩阵中的旋转部分，使用Matrix.CreateFromYawPitchRoll方法，传入适当的旋转角度。它确定了一个你可以用来级联"转换矩阵"和"比例矩阵"的"旋转矩阵"，来确定最终"世界变换矩阵"。
>
>





**补充说明：**中西方人在操作上存在一些不同的习惯，（当然这不全是因为中西差异决定的，个别人之间也存在习惯的问题。）旋转的控制上也许会不同，所以很多游戏都设置了"反转X轴"，"反转Y轴"这样的选项。
