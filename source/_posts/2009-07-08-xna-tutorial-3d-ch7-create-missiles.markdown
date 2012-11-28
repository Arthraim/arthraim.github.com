---
comments: true
date: 2009-07-08 15:38:32
layout: post
slug: xna-tutorial-3d-ch7-create-missiles
title: XNA教程-3D游戏-07-添加导弹
wordpress_id: 77
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---




在3D的地表上添加了可以控制的炮塔，接下去要让他向整个3D空间里自由的发射导弹。这次我们真正的要接触到Update这个方法了，另外知道在游戏循环中处理一些按键的逻辑。和2D一样，发射出去的导弹也是GameObject，那么一样的我们还是要用到它，并且要用到它的集合，来管理多枚屏幕中的导弹。




这一部分官方把视频分为了6段之多，不过其实内容基本上和2D差不多，那么还是一步一步看下去，其实分的越细致，越容易理解的。




**第一步、扩展GameObject**




正如前面说到的，这一次要用GameObject来描述导弹，那么它就应该具有所有导弹需要的属性，和2D类似，需要的是一个Vector3记录初速度， 和一个bool记录模型是否存活。




添加这样两行到GameObject类中～




    public Vector3 velocity = Vector3.Zero;
    public bool alive = false;






  * velocity：Vector3，记录速度（速率和方向），初始化为0；


  * alive：bool，记录是否存活，初始化为false。




* * *







**第二步、添加missiles数组**




拥有了为导弹扩展之后的GameObject类，现在就要添加一定数量的导弹了，所以我们用到了一个GameObject数组，来记录所有游戏中出现及没有出现的导弹。




**1、声明：**还是要先添加声明，一个const int记录数组大小，即最多的同屏导弹数。在Game1类的头部添加代码：




    const int numMissiles = 20;
    GameObject[] missiles;




**2、初始化：**在LoadContent中用循环来初始化数组中所有的GameObject。找到LoadContent()：




    missiles = new GameObject[numMissiles];
    for (int i = 0; i < numMissiles; i++)
    {
        missiles[i] = new GameObject();
        missiles[i].model = Content.Load<Model>(
        @"Modelsmissile");
        missiles[i].scale = 3.0f;
    }




实例化missiles数组为20个GameObject。




循环使用for，遍历每一个，并实例化 ，载入模型为missile，缩放大小为3倍，因为模型比较小的缘故吧。




* * *







**第三步、导弹发射输入**




这一步要编写控制导弹发射的逻辑，和2D一模一样，除了命名不一样。一样从声明和Update代码两部分看。




**1、声明：**




    GamePadState previousState;
    #if !XBOX
    KeyboardState previousKeyboardState;
    #endif




#if和#endif在之前控制炮台转动的教程中已经出现过。




这里为什么用到这样两个变量，在文章最后为加上翻译的资料。




**2、Update代码：**




    missileLauncherHead.rotation.Y = MathHelper.Clamp(
    missileLauncherHead.rotation.Y,
    -MathHelper.PiOver4,
    MathHelper.PiOver4);
    missileLauncherHead.rotation.X = MathHelper.Clamp(
        missileLauncherHead.rotation.X,
        0,
        MathHelper.PiOver4);
    if (gamePadState.Buttons.A == ButtonState.Pressed &&
        previousState.Buttons.A == ButtonState.Released)
    {
        FireMissile();
    }
    #if !XBOX
    if(keyboardState.IsKeyDown(Keys.Space) &&
        previousKeyboardState.IsKeyUp(Keys.Space))
    {
        FireMissile();
    }
    #endif
    previousState = gamePadState;
    #if !XBOX
    previousKeyboardState = keyboardState;
    #endif




这里的代码看上去挺多，但逻辑其实很清晰。都是在上一次按键放下的前提下，看下的话，就调用FireMissile方法。这样就不能按住案件连续发射了。




FireMissile方法正是下一步要完成的内容。




* * *







**第四步、发射导弹**




在Update方法里的FireMissile就是更新导弹的一个重要步骤，我们应该在这里让导弹活过来，并且给予初速度。




这里不向2D获取速度、位置那么简单，因为要知道我们是三维的，所以我们用了一些新的方法来得到我们想要的，看下去吧。




**1、完成FireMissile方法。**




    void FireMissile()
    {
        foreach (GameObject missile in missiles)
        {
            if (!missile.alive)
            {
                missile.velocity = GetMissileMuzzleVelocity();
                missile.position = GetMissileMuzzlePosition();
                missile.rotation = missileLauncherHead.rotation;
                missile.alive = true;
                break;
            }
        }
    }




我就说微软工程师都是天才，先直接写了FireMissile方法，然后现在又无中生有了两个方法。不去管它，逻辑比较简单，foreach遍历所有的数组中的GameObject，如果不存活就让他活过来。




要活过来的话，先要给速度，再给初始位置，再给初始旋转为炮塔转到的角度。然后设置alive为真，break跳出。跳出保证每次只发射一颗子弹。




**2、声明两个新变量。**




这里我们需要声明两个新的变量，在Game1头部加上两行代码：




    const float launcherHeadMuzzleOffset = 20.0f;
    const float missilePower = 20.0f;




两个浮点的常量。命名有些奇怪，但其实意思很简单






  * launcherHeadMuzzleOffset，发射器头部炮口偏移量，翻译过来就是这个意思，就是指炮管的长度，也就是导弹刚刚射出的时候距离模型中心的长度。这个值得问做模型的美工啦～


  * missilePower，导弹的威力，就是指每一次更新导弹的位移，也就是速率。导弹的威力取决于速度？也许在这个游戏中是的吧。




这是一个准备工作，为的是完成天才们预留的两个方法。




**3、GetMissileMuzzleVelocity方法。**




    Vector3 GetMissileMuzzleVelocity()
    {
        Matrix rotationMatrix =
             Matrix.CreateFromYawPitchRoll(
                 missileLauncherHead.rotation.Y,
                 missileLauncherHead.rotation.X,
                 0);
        return Vector3.Normalize(
            Vector3.Transform(Vector3.Forward,
            rotationMatrix)) * missilePower;
    }




这里首先创建了一个rotationMatrix，用的就是CreateFromYawPitchRoll，之前也使用到过。从初始化的代码来看，得到的就是炮台的旋转情况，因为Z轴的旋转，也就是翻转（roll） 是不存在的，所以传给其0就可以了。




之后的步骤有些复杂，拆开来看。






  * 首先是调用了Vector3.Transform()这个方法，这个方法是把一个旋转矩阵转换成一个向量。


    * 第一个参数为一个Vector3，也就是参考的方向向量。


    * 第二个参数是一个Matrix，也就是旋转矩阵。







方法会得到相对于第一个参数的向量的，当前Matrix的旋转情况。






  * 其次是调用了Vector3.Normalize这个方法，这个方法是把一个向量转换成一个单位向量。唯一的参数就是一个Vector3。相当于只保留了向量对方向的表达性，而去掉了速率的表达性。这样为最后一个工作作了铺垫。


  * 最后把单位向量乘上missilePower，也就是位移量，这样给返回的Vector3加上了速率的信息。




得到Velocity的过程就是这样，有了方向和速率，速度就这样完整了。




**4、GetMissileMuzzlePosition方法。**




得到了速度，这里要给他一个位置信息。因为是在FireMissile方法中被调用，那么其实就是起始位置，所以来看看代码。




    Vector3 GetMissileMuzzlePosition()
    {
        return missileLauncherHead.position +
            (Vector3.Normalize(
                GetMissileMuzzleVelocity()) *
                launcherHeadMuzzleOffset);
    }




很简单的直接返回。一样是一句复杂的语句，拆开来看看。






  * 首先调用了GetMissileMuzzleVelocity()方法，得到上一个方法完成的速度Vector3。


  * 然后调用了Vector3.Normalize()方法，只取得它的方向信息，去除速率信息。


  * 最后乘上launcherHeadMuzzleOffset，使得导弹出现的第一个位置是炮管的顶端，而不是模型的中心。




意思很明了，前两个调用相当于是得到炮塔在按键的一刹那的方向。换句话说，用GetMissileMuzzleVelocity方法里的逻辑也不难得到。




* * *







**第五步、更新导弹**




因为3D的缘故，第四步稍稍显得有些复杂了，有一些新的方法。




当然在完成了这些工作后，现在就显得简单的多了，这一步，只要让导弹在每次更新是移动一定的距离，并且在移动到一定距离后就消失。




**1、编写一个UpdateMissile方法。**




首先要写一个UpdateMissile的方法来管理所有的Missile的移动。




    void UpdateMissiles()
    {
        foreach (GameObject missile in missiles)
        {
            if (missile.alive)
            {
                missile.position += missile.velocity;
                if (missile.position.Z < -6000.0f)
                {
                    missile.alive = false;
                }
            }
        }
    }






  * 不需要返回值。


  * 遍历所有的导弹，如果它存活着就更新他的位置。


  * 原来的位置加上速度就是现在的位置。


  * 如果导弹的Z坐标小于-6000，即，深入屏幕6000，那么就让他死掉，准备再一次的发射。




以上就是所有的逻辑。




**2、调用UpdateMissile方法。**




写完方法当然要调用（这次到不是先调用再写了）。在Update方法中，previousState = gamePadState;这一行之前插入一行代码。




>

>
> UpdateMissiles();
>
>





就是这样。




* * *







**第六步、绘制导弹**




最后一步每次都是这样子，就是完成之前逻辑后，把它画出来，习惯了这系列的教程就非常明白了。




相对于前面调用DrawGameObject来说，这一次只是要用一个foreach循环调用很多遍而已。




    foreach (GameObject missile in missiles)
    {
        if (missile.alive)
        {
            DrawGameObject(missile);
        }
    }







最后，编译，运行～(点击放大，导弹太小，难以察觉，不如调整一下参数啊～)




[![](/images/uploads/zb/2009-07-08_runtime.jpg)](/images/uploads/zb/2009-07-08_runtime.jpg)




【官方源代码下载】




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=162)




* * *







之前说过，处理输入的那一步存在的一些疑问。（其实2D里也一样吧～）为什么要记录上一步的操作状态。究竟起到了什么作用，那么这一段解释非常非常的清楚。




（以下文字翻译自XNA Create Club网站，未经允许禁止转载。由于本人水平有限，所以有任何错误希望高手更正。）




**什么是上一步操作状态，为什么需要记录它？**




>

>
> 要理解对应游戏的输入，很有必要退一步看看更加普通的输入。控制器的输入信号一般只是简单的电流信号，它被一种称为驱动软件的低级软件系统分析解释。信号以一定的频率传入分析它们的软件。
>
>

>
> 有些软件把输入的解析表现出来，反映出很多用户使用这个输入设备做出的动作，并且把这些动作存入缓冲记忆体中等待被读取或释放。这种类型的输入处理被称为"缓冲模式输入"(buffered-mode input)。
>
>

>
> 另一种输入处理不使用缓冲。取而代之的，是提供索取信号是的当前状态。没有提供参数来标识一定时间内所做出的特定动作。取而代之的，是程序员可以在任何时间自由的访问控制器的当前状态，包括所有的轴的位置以及按键的情况。程序员可以自由存储和操作相关的信息。这种输入处理被称为"立即模式输入"( immediate-mode input)。
>
>

>
> XNA Framework使用立即模式输入，并且支持全部三种输入方式----Xbox360控制器，键盘以及鼠标。这意味着，对于任意一种输入设备，你都可以在任何时间查询输入设备的当前状态信息。然而，你却不能查询任何过去的设备状态，除非你明确的把它存储在其他地方。
>
>

>
> 为什么要查询过去的输入状态呢？举个强烈依赖玩家快速按键的游戏作为例子。在这样的游戏中，长按按键不会有什么效果，只有重复的连打才能奏效。
>
>

>
> 由于立即模式输入处理的这一特性，每一次输入状态被查询，一个长按的按钮表现为"被按下"（Pressed）。不检查过去的状态是否是"被释放"（Released），就没有办法来区分按键是刚刚被按下还是长按了一段时间。
>
>

>
> 存储上一次状态缓解了这个问题。每一次循环到达更新（Update），正常的处理输入，但是在循环的末尾，存储当前状态为一个变量。这个变量就成为上一次状态。要检查按键是否是被一次按下还不是长按着，检查当前按键状态为"已按下"(Pressed)，并且上一次按键状态为"已释放"（Released）。
>
>

