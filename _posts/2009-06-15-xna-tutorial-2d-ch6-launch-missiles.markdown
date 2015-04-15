---
comments: true
date: 2009-06-15 11:23:44
layout: post
slug: xna-tutorial-2d-ch6-launch-missiles
title: XNA教程-2D游戏-06-发射炮弹
wordpress_id: 66
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---




**XNA教程 -- 2D游戏 -- 06 -- 发射炮弹**




之前已经完成了很多的工作，创建了GameObject类，实例化了加农炮，并且在屏幕上画出了背景和加农炮。而且加农炮已经拥有了Update的代码，即玩家可以控制这个加农炮了。只是，距离我们的要求，这还远远不够，我们的子弹在哪里呢？加农炮打出来的炮弹呢？没错这就是这一次要完成的。我们要创建能被你发射，并且按方向穿过屏幕的炮弹了。




这一次的工作可谓相对有些复杂了，无论是代码量，还是概念，还是数学知识的要求都有了一定的提升。不过不用慌，我们还是跟着视频一点一点的做下来，最终我们一定能理解只是，并且做好这个东西的。OK，直接进入正题。




* * *







**第一步、修改GameObject**




之前说过了GameObject的作用，是用来实例化不同的游戏对象的，那么他的模型应该够健壮，能够实例化各种各样的现实对象。按照我个人的习惯，当然是一个对象一个类，不过既然视频是这么处理，还是跟着做。这里我们要做的很简单，找出之前的GameObject类，我们要添加两个成员变量。




速率：这个速率是指炮弹具有的速度，拥有了这个速度，我们才能慢慢的把它平滑的在画面上移动。不理解没关系，先看下去。




生存：是否生存，我的炮弹如果出了屏幕，那么它就已经消失了，或者说死了，所以这里有个boolean类型的值记录生存或死亡。




当然，这个类也会生成cannon，所以这里可以看一下，加农炮的速率是什么？速率是位移 / 时间，我们的加农炮只有旋转没有位移，很好就是0。再看生存，死亡，它应该一直存在，所以一直是true就好。那么接下来看代码。




先声明。




    public Vector2 velocity;
    public bool alive;





然后在构造方法里初始化。




    velocity = Vector2.Zero;
    alive = false;





好了，这就是我们要在第一步里完成的全部工作，不用疑惑，接下去做，就知道会发生什么了。




* * *







**第二步、创建Cannonball数组**




这里我们要完成的工作是建立一个数组，放上所有可能被发射的炮弹的GameObject，然后把它们初始化。




首先找到Game1.cs，你创建背景和Cannon的地方，在那些代码下面声明两个新的东西。




    const int maxCannonBalls = 3;
    GameObject[] cannonBalls;





解释一下，第一个带有const修饰符的是一个常量，maxCannonBalls。意思很明白，就是屏幕上最多出现的子弹数，因为子弹过多会使得update拖慢，那么能进行的draw就很少，所以一上来我们定义成3比较安全。如果过多，可以想见，会出现一帧一帧很卡的效果，像幻灯片一样。而如果加入了一些安全机制，那么就变成了跳帧现象，就是不再计算和绘制。之后一个GameObject[]的数组，就是放无数GameObject用的，至于放多少，是看初始化的。好吧，下面看初始化代码。




找到LoadContent方法，在cannon的相关后面加上以下代码。




    cannonBalls = new GameObject[maxCannonBalls];
    for (int i = 0; i < maxCannonBalls; i++)
    {
        cannonBalls[i] = new GameObject(Content.Load<Texture2D>(
            @"Spritescannonball"));
    }





如果你熟悉C#的话看懂这个没问题，无非就是循环一下，初始化一下。其实用List<>的效率肯定要高一下。但是这里是不可以使用foreach的，因为foreach肯定要求所有的对象都已经实例化了，而我们的工作就是在实例化这些对象。恩，其实这一步已经完成了，这些代码和载入其他Content都是类似的，只是这里遍历了3次而已。下一步吧！




* * *







**第三步、更新cannonball的位置**




这一步要更新cannonBalls的每一个ball的位置，当然这里就要用到我们的速度了。只要每一次更新的时候，都加上速率值（一个向量，叫速度更合适，速度是有方向的速率没有），那么它就会自动更新到新的位置了，如果在这个时候把它画出来，就可以看到它在一帧一帧的变换中变换着自己的位置，平滑的移动了一条直线（或其他轨迹，取决于速率或者更像是速度的这个值）。我们把要更新的代码分出一个方法来，叫做UpdateCannonBalls()，如果你坚持，也可以把它们写在Update的方法里，谁都没有强制你。看代码：




    protected override void Update(GameTime gameTime)
    {
        //...Other code
        UpdateCannonBalls();
        base.Update(gameTime); //必须在这一行代码之前
    }
    public void UpdateCannonBalls()
    {
        foreach (GameObject ball in cannonBalls)
        {
            if (ball.alive)
            {
                 ball.position += ball.velocity;
            }
        }
    }





上面是Update方法里的修改，其他代码不去管它，在base.Update(gameTime);之前加上就好。至于下面的方法，就是更新了，放在Game1类中的任何位置都是没有关系的，只要是别的方法的外面，Game类的里面。这里采用了foreach的方法，每一次做一个校验，如果还alive，活着，那么就给他的位置加上速度向量。没错就是这个逻辑。这一步就是这样子。




* * *







**第四步、发射cannonBall**




这一步要完成的工作就是发射炮弹了，要知道我们的初始向量才是关键。当然，要知道按键的那一刻，才能确定初始向量。那么首先要做的就是确定按键的相关操作。这里我们需要先声明两个变量。




    protected override void Update(GameTime gameTime)
    GamePadState previousGamePadState = GamePad.GetState(PlayerIndex.One);
    KeyboardState previousKeyboardState = Keyboard.GetState();





它们会被用来记录上一次Update时候的按键状态，这是为什么呢？看下面的代码。它们中，第一个if语句被添加在Update方法中，你之前完成的cannon.rotation += gamePadState.ThumbSticks.Left.X * 0.1f;这一行的后面。而第二个if语句，放在#if !XBOX和#endif之间的区域内，#endif的上方就可以。当然其实顺序并不是那么重要。




    protected override void Update(GameTime gameTime)
    if (gamePadState.Buttons.A == ButtonState.Pressed &&
        previousGamePadState.Buttons.A == ButtonState.Released)
    {
        FireCannonBall();
    }
    if (keyboardState.IsKeyDown(Keys.Space) &&
        previousKeyboardState.IsKeyUp(Keys.Space))
    {
        FireCannonBall();
    }





解释一下，第一个if是用来检查是否按下X360的手柄上的A键。上一次没有按键，而这一次按键的情况下，即刚刚按键的一刹那，我们实施FireCannonBall()这个方法。而连续按住则不会连续的出发这个方法。另外一个if则是键盘的操作，一样的，只有按下键盘上空格键的那一刹那才会触发。这是操作，究竟触发了，调用了FireCannonBall这个方法要做什么，来看它的实现。




    public void FireCannonBall()
    {
        foreach (GameObject ball in cannonBalls)
        {
            if (!ball.alive)
            {
                ball.alive = true;
                ball.position = cannon.position - ball.center;
                ball.velocity = new Vector2(
                    (float)Math.Cos(cannon.rotation),
                    (float)Math.Sin(cannon.rotation)) * 5.0f;
                return;
            }
        }
    }





来分析下代码，首先，我们遍历三个ball，因为最多只有3个，所以用这样的方法就好了（否则应该加上索引什么的，这里不讨论算法），看哪个球还活着，哪个球死了，只有死了的炮弹，我们才能让他再一次活过来，否则就要等到它死掉为止。那么显然，if(!ball.alive)校验后，留下来的就是可以活过来的炮弹，于是我们让他活过来。






  1. 首先设置它的alive属性为ture，证明它活过来了。


  2. 然后设置初始化的位置，是加农炮的位置减去炮弹的中心向量。（这个也许视频里直接ball.position = cannon.position了，那样子会错位）


  3. 然后要设置它的速度了，方向就是当前加农炮的角度，Cos(A)和Sin(A)计算出单位1的向量确定方向，然后乘上5倍使它得到速率。这样一个速度就初始化完成了，这样每一个炮弹会沿着加农炮发射的方向前进而不管你后面的，前面的炮弹如何。至于数学计算，后面会翻译相关资料说明。但其实很容易理解，画个半径为1的圆，然后连成一个三角形，用Sin,Cos就很容易理解了。数学知识。




还有一个重要的步骤，就是我们还需要让每一次Update后，把当前的键盘（或手柄）状态存入为上一次的键盘（或手柄）状态。代码只有两行。那个标记就不用再多做什么解释了吧，键盘的操作都包含在这个标记里，只是这次我们要重新写一个而已。把这个代码紧贴着base.Update(gameTime);的上方就可以了。




    previousGamePadState = gamePadState;
    #if !XBOX
    previousKeyboardState = keyboardState;
    #endif





这样，我们炮弹的发射就做完了，每一次按键，就会把已经死掉的炮弹重新发射出来，否则就不发射。很容易理解～下一步还没有画，我们要把发出去的杀掉，否则不是我们整个游戏从头到尾只能发射三次吗？




* * *







**第五步、杀掉cannonball**




就像刚才说的，游戏从头至尾只能发3个炮弹就太可怕了，它们按照牛顿惯性定律按照初速度朝着遥远的宇宙和未来飞去了。所以，我们要在它们飞出屏幕看不见的那一刹那，就把它们杀掉，资源回收～ 当然杀掉的策略可以有很多，比如碰撞了什么东西（比如将来的UFO），比如限定一定的时间，比如我们使用的飞出屏幕。因为飞出屏幕最符合逻辑，且只需要我们已经定义的这些信息就可以实现，所以采用这个策略。




在UpdateCannonBalls这个方法里，找到ball.position += ball.velocity;这一行，在其后面加上如下代码：




    if(!viewportRect.Contains(new Point(
        (int)ball.position.X,
        (int)ball.position.Y)))
    {
        ball.alive = false;
        continue;
    }





翻译成中文就是：如果ball所在的位置，转换成一个点，且这个点没有包含在代表了整个屏幕大小的矩形viewportRect当中，那么就把alive改成false，换言之杀掉它。杀掉后继续循环，生怕还有别的一起死掉而漏掉了。




很清晰很明白，就这么简单的三行代码，我们把飞出屏幕的炮弹杀掉了。OK，接下去最激动人心的时刻就要到了。话说微软的教程还真奇怪，一般自己写游戏都会是加一点逻辑，画一点逻辑的吧，他这个逻辑也太强了，全部想明白了再画，多辛苦啊。当然人家肯定是现成有了代码才这么干的吧。




* * *







**第六步、画出cannonball**




要画出这写炮弹，当然就是跑到Draw方法里面去了，有了之前的经验，我们应该很容易画。只是要注意的是画完背景先画炮弹，再画炮，这样感觉炮弹是从炮筒里出来的，不然很傻哦。




    foreach (GameObject ball in cannonBalls)
    {
        if (ball.alive)
        {
            spriteBatch.Draw(
            ball.sprite,
            ball.position,
            Color.White);
        }
    }





OK这就是绘制的代码，现在编译运行一下看看效果吧。（在我运行之后感到非常不满足，于是我把子弹数字稍微的改了一下，截图如下，嘿嘿～）




[![](/images/uploads/zb/2009-06-15_Chapter6Runtime.jpg)](/images/uploads/zb/2009-06-15_Chapter6Runtime.jpg)




【官方源代码下载】




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=153)




* * *







**翻译一、使用数组**




这是属于第二步的信息，教你使用数组，属于C#语法范围，其实没有什么特别的意义。权且翻译在这里。（以下翻译由本人翻译自[这里](http://creators.xna.com/en-US/education/gettingstarted/bg2d/details/6_2)，未经允许请勿转载，转载请注明译者及翻译出处）




>

>
> 教程到目前为止，你已经能熟练的使用一些预定义的类，并且为了能封装一些有规律的使用的数据，能创建一些自己的类了。
>
>

>
> 但是还有另外一种数据组织形式你需要知道并且学会使用。你也许需要使用大量相似的对象或足够相似的对象，事实上，它们都是同一类型的类。它们可能是GameObject，Texture2D对象，也可能是其他对象。你需要一些这样的对象，并且希望有一个相似的操作方式来操作它们。
>
>

>
> 例如，如果你有一组要绘制到屏幕上的GameObject对象，并且他们中的每一个都有自己的位置和Texture2D，你为他们每一个写的代码都会是完全相同。虽然目前是这样，但是你还是会要写一些一样的代码来处理每个 GameObject，使你的代码变得混乱。有什么办法只需简单的让对象的集合运行一样的代码，并且可以让每一个都拥有同样方式的互动呢？
>
>

>
> 有 ---- 解决方案就在于数组。一个数组是一个相似对象的线性集合。你可以拥有一个人和类型的数组；一个Vector数组，一个GameObject数组，一个字符串数组，或者其他任何可能并且合法的数组。
>
>

>
> ![](/images/uploads/zb/BG_3.6.2.1pd.png)
>
>

>
> 这张图显示了创建有3个GameObject元素的数组的代码顺序。你使用表明数组元素类型的类型名之后加上[]来声明一个数组。之后你使用new和[x]来初始化数组，x是你数组元素的数量。
>
>

>
> 如果数组中的数据类型为一个类，你必须循环遍历这个数组并且初始化每一个数组元素存储的对象。
>
>

>
> 一旦完成这些工作，你可以使用数组名加上[x]的方式访问数组中的每个元素，x代表了你要访问的元素，0开始代表第一个元素。因为这个指数的开始是0，调用cannonBalls[1].position会给你数组中第二个GameObject对象的position变量。
>
>

>
> 一个更加强大的方式来访问数组是循环，在上面的图中已经表现出来。一个for循环允许你针对所有数组中的元素运行相同的代码。你也可以使用一个foreach循环，它将被在后面的步骤中表现出来。两种方法都允许你让多个同类元素运行相同的代码。这也帮助你对已写代码的重用，而不用写更多的代码，即便数组的大小在今后得到扩展。
>
>








**翻译二、转换一个旋转角度为速度向量。**




这是属于第四步的信息，属于数学知识，可以帮助不懂这个数学原理的朋友理解那部分代码，翻译在这里。（以下翻译由本人翻译自[这里](http://creators.xna.com/en-US/education/gettingstarted/bg2d/details/6_4)，未经允许请勿转载，转载请注明译者及翻译出处）




>

>
> 学会这个教程的重点，你已经学会了采用弧度来传递旋转角度。然而，发射炮弹的动作需要我们使用Vector2来传递cannonball的动作。
>
>

>
> Cannonball的动作应当和加农炮的角度有联系 ---- 当你的加农炮转到垂直向上，你的炮弹应该垂直向上飞。同样的，当你的加农炮转到平行于地面的位置，炮弹应该平行于地面飞行。
>
>

>
> 来做出确定这个数据的唯一方法就是通过加农炮旋转角度的弧度。显然，你必须做一些转换工作，把旋转角度转换为给予炮弹的向量。
>
>

>
> ![](/images/uploads/zb/BG_3.6.4.1pd.png)
>
>

>
> 一个Vector2有两个组成部分：X坐标和Y坐标。在这个2D游戏的最终目标中，炮弹的X坐标在横轴上有怎样的动作，炮弹的Y坐标在纵轴上也有怎么样的动作。
>
>

>
> 在三角中，有个灯饰我们可以用来从一个角构造成一个vector。在如图所示的单位圆中，任何一个向量V可以被想象为拥有特定的从0开始的角θ。这个角度相当于我们想法中的旋转角度。为了得到向量V，也就是炮弹的速度向量，我们尝试着去发现，你可以使用下面的规则从θ中得到X和Y.
>
>

>
> V = (cos(θ), sin(θ))
>
>

>
> 这些数学方程式,sine和cosine,代表任何一个有θ大小的角的X和Y的数据。为了构造一个2D向量,你只需要知道X和Y.所以sine和cosine方程就是我们想要的.向量的X坐标是cos(θ)，而向量的Y坐标是sin(θ)。
>
>

>
> 一个向量的方向使用Y和X坐标的关系来表现。（成为斜度）但是向量也具有幅度。从我们的炮弹上说，它决定了多快的速度来接近向量所指向的方向。在我们的代码中，我们使用一个常数去乘向量的方式来增长这个幅度。它同时使用相同的值去乘了X和Y坐标，确保斜度不变，但向量变得更长了 ---- 这样，炮弹移动加快了。
>
>





That's all, finished~ Good luck ;)
