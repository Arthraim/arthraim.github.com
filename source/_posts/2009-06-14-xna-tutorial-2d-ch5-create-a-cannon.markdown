---
comments: true
date: 2009-06-14 19:56:33
layout: post
slug: xna-tutorial-2d-ch5-create-a-cannon
title: XNA教程-2D游戏-05-添加一门加农炮
wordpress_id: 65
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---




**XNA教程 -- 2D游戏 -- 05 -- 添加一门加农炮**




来到第五章，这一次要完成的是：建立一个类掌握你所有的游戏数据，并建立你第一个Game Object（游戏对象？还是不翻译比较好）---- 一个用户控制的加农炮，使用Xbox360手柄或者是PC键盘作为输入。完成了这一次的事情，整个程序就有交互了，就更加接近一个游戏了，其实游戏说穿了不就是一个有交互的幻灯片吗。




之前我们做过的东西都是一样的，所以这里就直接开始说真正的步骤了。直接看到此文的同学，相关文章里面应该可以找到之前的那些文章，从第一篇开始看吧，我想等我写完之后，会整理一个目录出来的。




在做所有的工作之前，我们要了解一个概念，就是屏幕的向量。一般做其他游戏的时候，包括direct3D也一样，屏幕的坐标是左上角的点为原点的。X轴正方向在朝右边，而Y轴正方向朝向下边。但是在XNA中也是这样。另外，角度却不是这个坐标系，而是右边是X轴正方向，上边才是Y轴正方向，原点因为是角度当然就无所谓了。这是说明，请大家不要被positon和rotation采取不一样的坐标系而搞混了。




**第一步 ---- 建立我们的GameObject，在Game中添加这个GameObject**




GameObject是什么，究竟为什么要添加这么一个GameObject在说完具体的代码方法之后我会说，先讲直接上手的东西，因为可能有人对于概念不太感冒。我们要做的工作的前提是，你已经成功的画上了背景，那么接下去我们要加上的加农炮，让我们先添加一个GameObject吧




**1、添加GameObject**




首先就是要新建一个类，作为GameObject，具体的步骤如下。




**1.1、新建一个类。**（中文版不好意思啦）






  * 右键单击你的Project选择Add中的New Item。（这里也可以直接选择Add，New Class）


  * 选中class，然后输入类的名称，方便讲解所以和视频一样，取名为GameObject.cs，确认。




（如下图，点击放大）




[![](/images/uploads/zb/2009-06-14_AddGameObject.jpg)](/images/uploads/zb/2009-06-14_AddGameObject.jpg)




**1.2、添加引用。**






  * 复制所有Game1.cs（如果你没改名字的话）的上面的using到新建的GameObject.cs文件的头部，OK




**1.3、添加属性。**




虽然个人认为还是添加为属性比较好，不过这里全都是用了Public的成员变量，那么权且这么用着，不讨论工程范畴的事情了。代码如下。




    public Texture2D sprite;
    public Vector2 position;
    public float rotation;
    public Vector2 center;





分别是一个Texture2D，我们全部要画的图像信息；Vector2，物体的具体位置；float，旋转的角度；Vector2，旋转中心的位置。有了这些信息，基本上一个2D游戏里能用到的物件的所有信息都在了，起码一个加农炮都在了。




**1.4、添加方法。**




这里我们只需要添加一个构造方法。代码如下：




    public GameObject(Texture2D loadedTexture)
    {
        rotation = 0.0f;
        position = Vector2.Zero;
        sprite = loadedTexture;
        center = new Vector2(sprite.Width / 2, sprite.Height / 2);
    }





传入一个Texture2D作为GameObject中的Texture2D sprite，并且初始化角度为0，位置为0，旋转中心为sprite的中心点。中心点可不是一定的，看下图，我们的中心点刚好是Sprite的二分之一高度二分之一宽度的地方，如果不是，那么要根据具体情况重新决定。




[![](/images/uploads/zb/2009-06-14_OriginalCannon.jpg)](/images/uploads/zb/2009-06-14_OriginalCannon.jpg)




**2、在Game1中载入加农炮。**




之后要在Game1中用到刚才的GameObject，并且把它初始化成加农炮。那么我们来吧，具体步骤如下。




**2.1、声明一个GameObject。**




在Game1.cs中，你声明了backgroundTexture的位置的附近加上新的声明代码，声明一个GameObject并且命名为cannon，这就是我们的加农炮了，具体代码如下。




    GameObject cannon;





**2.2、加载Content资源，并初始化位置。**




和背景的方法一样，让它读取我们的cannon.tga就好了，在Content Pipline中被自动命名为cannon（去掉后缀）的就是了。同样在你添加背景的LoadContent()方法，加入以下两行代码，载入资源，并把加农炮的初始位置放在距离屏幕右侧120像素，距离屏幕底端80像素的位置。




    cannon = new GameObject(Content.Load<Texture2D>(
        @"Spritescannon"));
    cannon.position = new Vector2(
        120, graphics.GraphicsDevice.Viewport.Height - 80);





至此，第一步就做完了，我们已经实现并且实例化了一个GameObject，并且把实例化的GameObject整成了cannon加农炮放到了需要的位置上，接下去就可以加入相应对它的操作了。




* * *




**第二步、添加玩家的控制，是加农炮可以旋转**




我们会使用Gamepad类和Keyboard类分别来接受Xbox360的手柄和PC键盘的控制。另外，我们用到一个MaxHelper类来实现简单的工作，而避免添加过多繁杂的代码。要做的依然很简单，所有的代码都在Update这个方法中完成，所以你只要找到Update方法，并且把代码加入到// TODO: Add your update logic here附近base.Update(gameTime);之前就好了。




**1、添加Xbox360手柄的控制。**




这个步骤相当的简单，我们可以看到在Update前部已经加上了一个类似的控制，就是为了使得游戏可以在Xbox360中退出的，可以关注一下它的实现，至于我们这里怎么做还要稍微讲一讲。




    GamePadState gamePadState = GamePad.GetState(PlayerIndex.One);
    cannon.rotation += gamePadState.ThumbSticks.Left.X * 0.1f;





如果你无视360，那么请直接看2好了，不过这里要稍微对这个代码解释一下。




GetState(PlayerIndex.One)是获取的操作是第一个玩家。因为采用无线手柄，所以X360会对玩家的手柄进行排序，最多四个玩家，也就是最多接4个手柄，分别为PlayIndex这个枚举类型下的One Two Three Four。这里也就是说默认的操作全部来自于One，就是所谓的1P，第一个玩家。




ThumbStick.Left就是XBOX左侧的类比遥感，这个东西不是简单的上下左右就好了，它是矢量的，就是说你打左的程度也会成为控制的一个信息，而不是简单的boolean。那么在这里的代码是怎么回事呢？首先，你要知道，ThumbSticks.Left.X这是一个Vector2（也重载了其他情况比如Vector3），也就是一个向量，相当于把摇杆所在的范围看作一个坐标系。这样一来这行代码的意思就非常的明显了，往左的程度越多×0.1f，加到角度（旋转角度也是一个向量）上，那么越往左就是越往逆时针旋转。这样理解下来的话，这句话的意思也可以写成：




    cannon.rotation -= gamePadState.ThumbSticks.Right.X * 0.1f;





这样其实我们控制的效果和键盘是不一样的，也就是说扳动类比遥感的角度越大，旋转速度就会越大。




**2、添加PC键盘的控制。**




相对于XBOX，这里的代码量要稍微多一点，不过反而好理解一点。只要读取左右按键是否按下，按下就相应的产生操作就好了。因为update是每一帧都会来读取的内容，所以虽然一次只读键盘一次，但是其实一直按住的连续操作也是会被接受的，不理解的朋友可以留言，因为初学者可能不知道update和draw两个方法在游戏主循环里会不停的调用的。代码如下：




    #if !XBOX
    KeyboardState keyboardState = Keyboard.GetState();
    if(keyboardState.IsKeyDown(Keys.Left))
    {
        cannon.rotation -= 0.1f;
    }
    if(keyboardState.IsKeyDown(Keys.Right))
    {
        cannon.rotation += 0.1f;
    }
    #endif





注意一下#if和#endif之前的代码，!XBOX的意思就是如果不是XBOX360的话，就会在编译的时候编译这一段代码，否则就不编译了，因为360是没有键盘的哦。




代码没有什么特别值得解释的东西。如果按键被按下，是左边就减去向量角度，是右边就加上向量角度。




**3、限制加农炮旋转角度。**




已经对加农炮的旋转进行了操作，可是还存在什么问题么？是的，现在我们的加农炮是可以360度回旋的，转啊转啊大风车一样，可是我们只能让他转在朝着屏幕右上角的90度里面怎么办？很简单，利用XNA提供的一个MathHelper类。名字取得很好听，"数学助手"，如果数学不过关那么就用它吧。不过这里我们只是限定一个范围，那么其实用if也完全可以完成，MathHelper帮我们减少了代码量，功德无量啊。代码也很简单，一行：




    cannon.rotation = MathHelper.Clamp(
    cannon.rotation, -MathHelper.PiOver2, 0);





加农炮的旋转角度来接收返回值。返回值一定不是最大值就是最小值了吧。




看看MathHelper.Clamp传入了什么？






  * **第一个参数**：需要被限定的变量，那么我们要限定的就是加农炮的旋转角度 ---- cannon.rotation。


  * **第二个参数**：最小值，我们旋转角度的最小值。显然，我们传入的最小的是-MathHelper.PiOver2，这有点像个宏，其实是MathHelper的另一个重要功能，帮我们记录一些常数。PiOver就是90度，Pi/2，这个学过向量应该知道，不知道的就可以补习数学去了。（下文会对这个东西详细的讲解）


  * **第三个参数**：最大值，就是0，就是说它最大的旋转角度在0，也就是水平线往右，X正方向，这个角度就可以了。




至此，我们第二步的工作就宣告完成了～ 接下去就是最令人兴奋的部分了，把我们已经加载的，并且可以操作的加农炮显示到屏幕上，把它画出来！其实我们它已经存在了，只是我们还看不见，所以，第三步就是让我们看见他！




* * *




**第三步、绘制加农炮**




无聊了，看了这么久写了这么久为什么运行的时候都不变？当然，因为没有画出来啊，接下来就是要在Draw方法里画了，方法也异常简单。




找到上次画了背景的那个地方， 也就是Draw方法，在画背景的那一行的下面加入如下代码。




    spriteBatch.Draw(
        cannon.sprite,
        cannon.position,
        null,
        Color.White,
        cannon.rotation,
        cannon.center,
        1.0f,
        SpriteEffects.None,
        0);





没错，其实和画背景一样，我们只是调用了SpriteBatch类的Draw方法，只是这一次我们使用了另外一个重载而已。只是这一次使用了参数比较多的那一个，一个一个的讲解过来吧。






  * **第一个参数**：Texture2D，也就是要画的纹理，这样我们全部要画的就是这个cannon.sprite这个Texture2D了。没错～


  * **第二个参数**：Vector2D，向量，也就是我们放的位置。我们传入了cannon.position，还记得吗？当时设定的是(120,高度-80)这个向量。


  * **第三个参数**：Rectangle，矩形，是指在我们的Texture2D上面的裁剪，有经验的一定知道，很多的动画都是通过这样的方式实现的，每一帧画在一张texture上，一次载入之后，以后每一次都只要变化这个参数来读取不一样的位置就好了。这里我们传入了null，意思就是默认为不进行任何的裁剪。可以尝试把第三个参数改成new Rectangle(0, cannon.sprite.Height / 2, cannon.sprite.Width, cannon.sprite.Height)绘制一半。


  * **第四个参数**：Color，传入颜色，和之前画背景一样，就是我们的色彩过滤。White代表不进行任何过滤。


  * **第五个参数**：float，传入的是旋转的值，它指的是向量的角度，就是屏幕的坐标系上的角度。我们传入的是0，那么就是X正方向，也就是不进行任何旋转，加农炮朝着屏幕左边。


  * **第六个参数**：Vector2，是图像的中心点的位置，这里要注意一下，这个中心会根据你绘制的图形重新定义，如果你画的东西不是整个的，那就会发生变化了。这里我们绘制了全部的内容，所以没问题。如果之前一样你只绘制一半，就惨了。也就是说在这个方法内部，是先切割，决定画的部分，再决定中心点，而不是先根据你传入的Texture2D决定中心点，再切割的。


  * **第七个参数**：float，这个是指图形缩放的倍数，如果要实现平滑的缩放，那么简单的Update里调整这个数值就可以了。我们这里的1.0f就是1倍，1 / 1，不缩放的意思。


  * **第八个参数**：SpriteEffects枚举，这个枚举也只有三个成员，FlipHorizontally，FlipVertically和None。也是2D游戏的除旋转、缩放意外非常重要的特效----翻转。一个是纵向的一个是横向的翻转。应该很好理解。我们传入None，就是不翻转。


  * **第九个参数**：Int32，也就是int了，这是指深度，0和1之间。2D游戏经常会有深度这个概念，Depth，往往，Sprite会自动对这些深度排序，这样可以实现一些角色挡住丛林，或者丛林挡住角色的变化。这是2D游戏很重要的一个参数。当然如果要让这个参数有效，你必须先设置SpriteSortMode，在这里就不多加讨论了，总之我们这里传入了0。




这样我们就画好了加农炮。运行一下吧，你已经在游戏上面做了一个可以转动的加农炮了。然后修改一下之前的一些代码看看有什么不同的变化～比如旋转的角度限定注释掉，炮会很可爱的。




[![](/images/uploads/zb/2009-06-14_RunTime.jpg)](/images/uploads/zb/2009-06-14_RunTime.jpg)







【官方源代码下载】




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=152)




* * *







之前也说到了，一些问题会在最后讨论一下，其实这就是两段在官网上也特别罗列出来要讲的东西，于是放上这两段的翻译。




**一、为什么要创建自己的类？**




我们在代码过程中，创建了一个GameObject，并且实例化为Cannon，可是为什么要自己创建一个GameObject呢？不是简单的把四个变量放到Game1里面就可以了吗？来看看下面这段翻译。（下文由本人翻译自[这里](http://creators.xna.com/en-US/education/gettingstarted/bg2d/details/5_1)，转载请注明翻译作者Arthraim，并表明出处，尊重我的劳动成功）




>

>
> [![](/images/uploads/zb/BG_3.5.1.1pd_a.png)](/images/uploads/zb/BG_3.5.1.1pd_a.png)
>
>

>
> 目前为止，你已经使用过了一段时间XNA Framework中的一些实现定义好的类（代码层面的对象，包含了数据和针对数据的操作的方法）了。
>
>

>
> 你已经修改了一些包含在Game1这个类中的一些方法，添加了一些新的类的声明，诸如SpriteBatch和Texture2D，并且在类中使用了他们。
>
>

>
> 然而，创建GameObject类完全是一种新的操作：创建一个你自己的包含了数据和方法的类的过程叫做定义一个类。乍一看，创建一个新的类文件，并且定义一些能像在Game1里面定义了一样，能够被简单的使用的数据和变量，也许需要非常多的工作。
>
>

>
> 例如，在GameObject中，一个被叫做position的数据成员Vector2。你可以简单的在Game1中定义一个包含位置数据的Vector2，因为一个就可以记录想让你追踪位置的对象了。但是还有旋转，速率，和其他活动的变量。当你考虑可以添加更多对象到屏幕上，而需要的都是一样的数据的时候，加入Game1的变量的数目就会开始向着无法工作的庞大的数据增长。
>
>

>
> 另外，如果你需要对这些数据进行一些运算操作，你就必须对每个变量采取相同的计算操作，一个接着一个。最终你的代码量将会非常的庞大并且没有效率。
>
>

>
> 类的目标是重用。综合一类你可能要用到多次的数据和方法到一个单独的"代码对象"，这样一个小单元，比一些独立的数据对象更加容易操作。
>
>

>
> 就像你可以在图中看到的一样，使用一个类，显著的减少了你需要在Game1.cs里要添加的声明，并且保持数据有序。当你要开始使用数列（相似并且可以按序操作的数据集合）声明一些对象，即游戏中对象变得更多的时候，这个概念会显得更加重要，没有类，数据会变得难以追踪。
>
>





其实这只是一个简单的面向对象的概念，不应该放到游戏编程这里来说，就像视频里其实没必要讲一些VS的用法，因为虽然讲了，但是根本对不会用VS的人没有帮助。一样的，对不懂OO的人来说，讲这么一段东西对于他们的思维来说也不能产生什么变化。所以说，无论是VS的用法，还是C#的语法，或是这里提到的OO思想，都是需要慢慢积累，或是在别的地方补课的，而不是通过这个简单的教程。




**二、Game Studio中的角**




（下文由本人翻译自[这里](http://creators.xna.com/en-US/education/gettingstarted/bg2d/details/5_2)，转载请注明翻译作者Arthraim，并表明出处，尊重我的劳动成功）




>

>
> 开发游戏中经常会需要你传递一个表示一个角的数值。2D对象的旋转只是你使用角的开始，还会有很多角的使用，比如在3D对象中，不同对象间反射的角，还有更多其他的。
>
>

>
> 有很多不同的方式去传递角。一个标准的方法就是用角度。意思是，把一个圆圈分为360个部分，每一个被称为一度。90度是四分之一个圆，180度是半个圆，等等。
>
>

>
> 然而，在计算机图形和XNA Game Studio中，更多的采用弧度来传递一个角。弧度采用和角度不一样的方法去切割一个圆 ---- 代替360度代表一个圆周的方法，是用2倍的π或2π代表一个圆周。这个值被存储在MathHelper类当中，只需简单的调用MathHelper.TwoPi。半圆则就是这个值的一半，或是简单的用π表示，也就是MathHelper.Pi。你可以继续细分来得到其他角度。90度则是MathHelper.PiOver2。
>
>

>
> [![](/images/uploads/zb/BG_3.5.2.1pd_a.png)](/images/uploads/zb/BG_3.5.2.1pd_a.png)
>
>

>
> 当使用SpriteBatch.Draw画一个Sprite，或者当你创建一些用来旋转的矩阵的时候，你需要弧度来传入旋转的值。但是如果你觉得角度更加方便的话，你可以在游戏中继续使用它，只要你在调用一些XNA Framework中需要传入弧度的方法的时候把它们转换成弧度就可以了。你可以使用MathHelper.ToRadians来完成这个工作，只要传入你要转换的角度即可。
>
>





这也是之前说到的会详细说的内容。最后我再补上一张图，方便大家学习吧。




[![](/images/uploads/zb/2009-06-14_CordinateSystem.jpg)](/images/uploads/zb/2009-06-14_CordinateSystem.jpg)




画是画的难看一点。






  * 我们在设置position的时候，使用的是最外面黄色箭头表示的这个坐标系的，就是X正方向朝左，Y正方向朝下。


  * 在设置center的时候，这个向量也是使用一样的方向的，只是坐标原点换成了Sprite的左上角，就比如我图例那个粉红色的框框的左上角（好像有些圆了 ;)）。


  * 而在设置初始旋转的时候，我们设置了0，那么就是像图例的弧度这张图一样，是摆正的。就是Y轴方向是和屏幕坐标系相反的。（但是弧度这个概念可是不受限于坐标系的，只是rotation这个参数默认的0度是在这样的一个坐标系里面的。）




好了我觉得该说的都说过了，那么就是这样。
