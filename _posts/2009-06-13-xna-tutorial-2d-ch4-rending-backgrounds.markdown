---
comments: true
date: 2009-06-13 16:35:48
layout: post
slug: xna-tutorial-2d-ch4-rending-backgrounds
title: XNA教程-2D游戏-04-绘制背景
wordpress_id: 64
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---




**XNA教程 -- 2D游戏 -- 04 -- 绘制背景**




通过前面三篇文章，大概可以知道做一个简单的小游戏的准备工作了。我们已经迈出了第一步。但是程序员们一定不高兴了，我们看了三篇文章，不但没有一行代码，还讲了VS怎么用，软件怎么安装这些无聊的事情。那么好吧，这一次我们要直接开始写一些代码，一些可以运行起来的代码。




在我们做这个之前，首先建立工程，并且把asset加入到content里面。完成的同学举手～是的我也已经完成了。那么，在做这个之前，还是要再讲一些东西 ---- Game1.cs。没错generate的文件中至关重要的就是Game1.cs这个东西，它在你的命名空间中自动生成了一个类，派生自Microsoft.Xna.Framework.Game，究竟Game类里实现了什么，你可以把dll找出来看看。（别告诉我.net的dll是不可读的，工具才是王道，哈哈）但是之前说了，微软还是奉行着不用关注底层的策略去做XNA的，所以大部分就不要关注了。




直接来看类的结构。有经验的人一定发现了，这个结构和directX的C#程序非常的接近，如果你和我一样用SharpDevelop直接新建过D3D的工程，那么一定看到过类似的代码。只不过仔细看看会发现很多东西不见了，也有些东西不能不见。比如熟悉的device变成了GraphicsDeviceManager。是换汤不换药吗？还是那句话，不要关注了。可能写完所有的教程我能讲讲。先大概讲一下：




他是个类，那么就具有一般类的所有的特性，比如与其他类进行数据交换等等。它代表了一个游戏，在XNA中Game类负责"开始"，"运行"，"绘制"（还是摧毁？没听清视频的话）一个游戏。它处理一些事物，例如"时钟计时"，"视频设备选择"，"清理"等。只要有这样一个类，就拥有了一个完整的游戏，用开始到运行到结束。




    public class Game1 : Microsoft.Xna.Framework.Game
    {
        GraphicsDeviceManager graphics;
        SpriteBatch spriteBatch;
        public Game1()    {    }
        protected override void Initialize()    {    }
        protected override void LoadContent()    {    }
        protected override void UnloadContent()    {    }
        protected override void Update(GameTime gameTime)    {    }
        protected override void Draw(GameTime gameTime)    {    }
    }





这个类实现我们两个变量和6个方法。




GraphicsDeviceManager类：是对device的封装，device的相关操作也可以通过这个类完成。

	SpriteBatch类：用以完成绘制2D纹理的相关操作。




Game1()不用说了，一个构造方法。




    public Game1()
    {
        graphics = new GraphicsDeviceManager(this);
        Content.RootDirectory = "Content";
    }





代码中实例化了GraphicsDeviceManager类，命名为graphics 。并且设置了Content管道的根地址为Content。这样一来，我们接下去在Content管道里可以调用的全部的asset都只能来自于Content目录中




    protected override void Initialize()
    {
        // TODO: Add your initialization logic here
        base.Initialize();
    }





覆盖了父类中的Initialize()方法，法如其名，就是用来初始化的。它允许游戏在运行之前做一些初始化的工作。可以在此查询任何需要的服务，和载入与图形无关的内容。

	base.Initialize()方法在被调用后可以通过任何组件枚举并初始化它们。




    protected override void LoadContent()
    {
        // Create a new SpriteBatch, which can be used to draw textures.
        spriteBatch = new SpriteBatch(GraphicsDevice);
        // TODO: use this.Content to load your game content here
    }





一样覆盖了父类中的LoadContent()方法。方法是用来加载content的。

	这个方法一个游戏只会被调用一次，并且是你加载所有content的地方。

	实例化了SpriteBatch类，命名为spriteBatch。




    protected override void UnloadContent()    {    }





这个方法一样简单，UnloadContent，就是释放内容的地方。

	这个方法同样只会被调用一次，并且是卸载所有已经载入的资源的地方。

	当前因为C#的托管机制，我们不用关注实例化的各种类的释放。




    protected override void Update(GameTime gameTime)
    {
        // Allows the game to exit
        if (GamePad.GetState(PlayerIndex.One).Buttons.Back == ButtonState.Pressed)
            this.Exit();
        // TODO: Add your update logic here
        base.Update(gameTime);
    }





Update方法相当于D3D中会被命名为FrameMove之类的方法，说白了图形上就是修改当前画面的地方。

	按照注释所说，他用以游戏执行一些逻辑，诸如更新画面，检查碰撞，收集输入以及播放音频。

	if (GamePad.GetState(PlayerIndex.One).Buttons.Back == ButtonState.Pressed) this.Exit();是退出代码

	base.Update(gameTime);自然就是调用更新的方法，一切工作要在它之前完成。




    protected override void Draw(GameTime gameTime)
    {
        GraphicsDevice.Clear(Color.CornflowerBlue);
        // TODO: Add your drawing code here
        base.Draw(gameTime);
    }





Draw方法相当于Paint和onPaint这样的方法了，是系统自动绘图的方法。应该很容易了解。

	GraphicsDevice.Clear(Color.CornflowerBlue);是清楚画面，现阶段试着改下颜色就会呈现不同的画面背景了。

	base.Draw(gameTime);则是绘制工作。




* * *




实际来开始写代码吧～




**第一步：声明一些东西**




我们声明一些我们需要的东西，在SpriteBatch spriteBatch;之前加入两行代码。




    Texture2D backgroundTexture;
    Rectangle viewportRect;





声明一个2D的纹理，加载之后将储存所有我们要画的东西。至于Rectangle矩形是一个数学概念上的类，用来确定屏幕上的绘制范围，无论画什么将画在这个区域内。




**第二步：加载asset**




我们要做的依然很傻瓜，就是在LoadContent中写一些载入asset（理解为载入文件多容易）的代码。LoadContent方法会在程序开始之前完成加载的工作，而你则需要把所有这个游戏会用到的东西在这里加载，这样才能在之后被调用。我们找到// TODO: use this.Content to load your game content here这行注释，就可以在它下面加上我们需要的以下代码了。




    backgroundTexture = Content.Load<Texture2D>(
        @"Spritesbackground");
    base.LoadContent();





正如大家看到的，这里只要直接用Content属性，然后告诉他路径就好了。.tga的后缀被省略了。调用base.LoadContent后资源载入成功。另外这里很高兴的看到了范型，写游戏的肯定很注意效率，那一定会担心范型的效率。不过我记得范型的效率有时候还高于现行数组等集合。




这里特别注意一下，按照视频中的GS2.0来看，会自动生成base.LoadContent();这行代码，留个心眼。




**第三步：绘制texture**




之前我们声明了texture并且载入了asset，那么接下去就是要coding了。要把它画在画面上，就要用到SpriteBatch类，在自动生成的代码中已经帮我们写好并且实例化好的类。它将帮助你把Scirpte画到你的设备上，PC或是X360，这就是为什么实例化SpriteBatch的时候传入了Device。我必须知道要画在哪里才可以。




接下去还是找到LoadContent方法，首先我们要确定绘图的范围，因为我们是画的背景，所以只要得到当前Device的viewport（视窗？）就可以知道这个范围。初始化这个范围到viewportRect这个矩形就好了，如参为四个，起始点的X坐标，Y坐标和宽度，长度。于是就是下面的一行代码。




    viewportRect = new Rectangle(0, 0,
    GraphicsDevice.Viewport.Width,
    GraphicsDevice.Viewport.Height);





意思很清晰，不过有个地方值得注意，视频中GraphicsDevice是通过graphics中的属性得到的，即是通过this，也就是当前的Game类实例化的GraphicDeviceManger中的属性得到的；而从3.1自动生成的代码来看，实例化ScriptBatch的时候可以直接使用this的GraphicsDevice属性。于是我们的代码也可以直接这么使用GraphicDevice属性，小细节而已，这里提到一下，于是继续。




接下来找到Draw方法绘制这一部分，找到// TODO: Add your drawing code here的地方，插入如下代码。




    spriteBatch.Begin(SpriteBlendMode.AlphaBlend);
    spriteBatch.Draw(backgroundTexture, viewportRect, Color.White);
    spriteBatch.End();





这段代码是2D游戏中才会出现的，在D3D里用sprite的时候也会看到类似的代码，beginscene等。SpriteBlendMode是个枚举类型，Additive和AlphaBlend和None三种。Additive就是叠加，等到后面有多个texture画上去的时候再做实验。Alphablend是alpha混合，就是半透明效果，带有半透明信息的图片可以直接贴上去而不需要你做额外的混合工作。在draw中传入要画的texture，要画的范围，以及最后一个颜色参数。这个颜色参数相当于CSS中的filter，就是一个滤镜，调整颜色的，这里传入白色代表不做任何过滤。最后end相当于ndScene，结束。




OK到这里代码就写完了，看一下总共我们只写了8行代码，就完成了背景的绘制，很简单，运行一下看看效果（点击放大），就收工啦～




[![](/images/uploads/zb/2009-06-13_DrewBackground.jpg)](/images/uploads/zb/2009-06-13_DrewBackground.jpg)




官方提供的源代码下载。




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=151)




* * *




接下去通过对网站上其他资源的翻译，达到对一些没有解释的内容的解释作用。




**一、Content**




Content是最莫名其妙的东西，我们莫名其妙的写出了Content.Load这样的方法，但是事实上，Content这个Game的属性是怎么回事我们根本不知道，里面有什么我们也不知道，那么究竟它是如何工作的我们先看一下我的翻译。（以下为本人根据微软提供的资料所做的翻译，原文请查看[这里](http://creators.xna.com/en-US/education/gettingstarted/bg2d/details/4_2)）




Content Pipeline是如何工作的？




>

>
> ![](/images/uploads/zb/BG_3.4.2.1pd.png)
>
>

>
> XNA Framework的ContentPipeline是XNA Game Studio非常重要的一个部分，目前为止所展现给你的那部分（添加物件到工程中以及调用和加载）只是对发生在幕后的事情的暗示。
>
>

>
> 在传统的游戏开发中，载入内容（转换内容文件到游戏在运行时可以使用的格式）是一件困难且耗时的工作。往往，程序员必须要从零开始编写一个载入系统（loading system），必须写一个导入器（importer）来消费3D和2D资源（诸如美工提供的模型和纹理），再写一个处理器（processor）转换这些导入的格式为代码对象，使得其他程序员可以这些代码对象进行操作，使模型显示出来并在游戏中产生互动。
>
>

>
> 然而在XNA Game Studio中，大量的这类工作已经由XNA Framework Content Pipeline完成了。Content Pipeline

		在你把一些资源放到解决方案查看器里的那一刻已经参与了进来。
>
>

>
> 如果你添加了一个Content Pipline能够识别的内容类型，你可以在解决方案查看器点击看到Content Pipline的属性。你会注意到任何一个内容都有四个属性。
>
>

>
> 第一个是Asset名字。这是一个字符串，用来在你调用ContentManager.Load的时候定义内容名称。注意他的命名就是文件名去掉了后缀。
>
>

>
> 第二个是Content导入器（importer）。它定义了从磁盘文件读取内容所使用的导入器的类型。一个导入器被调整接受某特定格式。只要文件一被导入，他会通过第三个道具----Content处理器（Processor）。这些处理器转换传入的数据到可以在运行时被读取的格式，并且转换成一个类。
>
>

>
> 任何独立的物件所对应的导入器和处理器可以通过第四个属性（名叫XNA Framework Content）的设置打开或关闭。如果设为true，这个内容就会完成之前提到过的导入和处理工作。如果设为false，Content Pipeline将不会导入或处理内容。
>
>

>
> 当你添加一个物件到解决方案查看器，这些设置将按照默认值设置，并且在大多数情况下，他们不需要修改。想要完成你会经常在游戏类中完成的事情----把内容加载到游戏中，所有你要做的事情就是调用ContentManager.Load方法了。
>
>





看完这段翻译有些东西不得不讲一下：




首先是属性，按照这里所说的属性是四个，并且Processor是没有扩展的。（如引用区域内的那张小图，此图为官网使用的示例图，不能放大）这是基于GS2.0的教程，但是在我现在使用的3.1版中我们根本没有这第四个属性，反而是像我的截图（下图，点击放大）里的一样，Processor拥有了很多的属性。




[![](/images/uploads/zb/2009-06-13_ContentPiplineProperty.jpg)](/images/uploads/zb/2009-06-13_ContentPiplineProperty.jpg)




其次就是ContentManager的解释，翻译里的描述的，我们采用的ContentManager类，其实不需要我们自己定义或实例化，在构造Game类的同时，这个ContentManager类已经作为Game类的一个属性被定义完成了，我们只需要直接调用(Game)this.Load<object>()方法就可以调用了。于是结合上面提到的GraphicsDeviceManager，我想，能不能也不要呢。于是我注释掉了两行代码，一个是声明一个是实例化，之后在运行到draw方法，需要用到this.GraphicsDevice类的时候提示：This property requires a graphics device service in the game service container. 这么说来ContentManager已经会自动被实例化，可是GraphicsDeviceManager却不可以。毕竟从这些代码上面来看，构造ContentManager只需要一个IServiceProvider作为如参，那么Game实例化过程中就可以完成这个操作。而GraphicsDeviceManager需要的入参是Game，这就不可能自动在构造Game的时候完成。（当然在我们看不到的地方是完全可以实现的）看来究竟内部如何实现的，又有什么考量，有深入研究的价值。




**二、sprites，sprite绘制，和spritebatch**




>

>
> ![](/images/uploads/zb/BG_3.4.3.1pd.png)
>
>

>
> Sprite（常译作精灵，此处不翻译）是游戏中显示的2D图形的常用名字。在过去，几乎所有的游戏是基于sprite方式的；2D图形曾是古老的处理器和电视游戏系统所能实时处理的全部。随着当前计算机和电视游戏主机中包含的图形处理但与（GPU）的降临，很多游戏转向了3D图形，但是在那些游戏当中，sprite仍然出现了，成为诸如烟和火这样一类的效果的首选方式，给予了他们不高的复杂度和较快的绘图速度。
>
>

>
> 在XNA Framework中，没有类似的Sprite类；一个XNA Framework制作的游戏中的一个sprite来自于很写Texture2D对象的结合，他们包含了要绘制的图像信息，和一个能设置图形设备让其正常绘制2D图形的SpriteBatch对象。
>
>

>
> 在3D游戏中，XNA Framework的绘制方法主要用来绘制3D模型到屏幕上。一系列的叫做绘制状态（render state）的图形设备的设置选项，按照默认设置的都是用来绘制3D模型的。SpriteBatch类处理转换这些绘制状态到允许2D图形的绘制。要做到这一点，SpriteBatch拥有两个方法，Begin和End。你必须分别调用它们让图形设备为2D图形做好准备，或者回复图形设备到3D绘制状态。
>
>

>
> 在调用Begin和End方法的中间，你可以调用SpriteBatch.Draw，并通过调整位置、大小、旋转、翻转、调色，以及更多的丰富多彩的选项来绘制Texture2D对象到屏幕上。
>
>

>
> 在调用Draw方法时，通过调整位置、大小、旋转、调色还有其他显示参数，你可以让一个单独的Texture2D对象表现出丰富多彩的不同的状态。利用随着时间的推移，平滑的变换这些参数，你可以模拟表情或其他动画效果。
>
>

>
> 一个标准的2D游戏，你只需要调用Begin一次，然后利用一系列的Draw方法的调用绘制所有的和游戏相关的sprite，然后再调用End。注意，在你的游戏中，你只需要一个SpriteBatch对象，无论你将要有多少Texture2D对象要画。
>
>

>
> 在教程的最后，你还会学到如何使用SpriteBatch来显示文本到屏幕上。这对于显示文本和数字信息给玩家来说非常的实用，就像得分，角色间的对话或者更多其它的东西。
>
>





可以总结下，一般2D游戏的Sprite就是Sprite，而XNA中的Sprite对象是不存在的，只有一个SpriteBatch对象，它不是真正意义上传统的Sprite，他是一系列texture纹理的集合。




其实这种实现方式在Direct3D9中就已经被使用了。人们在做2D游戏的时候（就像我当时找资料的时候）总是要回忆说DD7如何如何的好，如何如何的方便。于是有了利用DD7供给VB调用的组件实现C#调用DD7对象来实现2D游戏这样子极端的做法。其实在D3D8时代，去除directDraw的时候，就已经采用了类似XNA这样的2D游戏的实现方式。而D3D9中不但依然使用这种实现方式，还提供了directDraw7的.net的实现。




当然，如今在XNA中一定不会有directDraw了，不过我们应该已经能够接受这种2D游戏的开发方式了。只是话说回来，2D游戏还有多少市场呢？




终于结束Chapter4了，保证各位看过我的文章不用再去XNA Creator Club学习了。
