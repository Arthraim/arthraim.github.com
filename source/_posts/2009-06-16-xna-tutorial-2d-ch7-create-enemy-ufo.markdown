---
comments: true
date: 2009-06-16 11:24:54
layout: post
slug: xna-tutorial-2d-ch7-create-enemy-ufo
title: XNA教程-2D游戏-07-添加敌人UFO
wordpress_id: 67
categories:
- Programing
tags:
- 2D
- XNA
- 游戏开发
- 教程
---




**XNA教程 -- 2D游戏 -- 07 -- 添加敌人UFO**




这次要做的就是添加敌人 ---- UFO。我们有了一个防空武器，在一个可恶的看上去非常阴沉的外星球，那我们必须要有人来入侵，这才是建造军用设施的真正目的对吧。所以，程序员真是堪比创世纪的众神了。




加入UFO的方法其实和子弹差不多（记住这句话，因为我后面实在是懒得讲了），它需要它的初速度向量，需要被杀掉，最后需要画在屏幕上，如果稍微改一下，我们完全可以不看教程做到了。添加UFO还涉及到了一个新的概念，就是随机数，游戏中随机是非常重要的东西，几乎没有一个游戏不用到随机概念的，前段时间刚刚过了25岁生日的俄罗斯方块也是。随机，是游戏的灵魂，而我们将用到Random来产生随机数，再使用XNA提供的方法限定我们需要的范围。OK，开始吧。




**第一步、创建UFO**




我们要声明很多的变量，可能一时半会儿不会知道有什么用。一样的，找到你声明了其他东西的地方，加农炮啦，炮弹啦，手柄和键盘的状态啦之类的，Game类的首部，添加以下代码，代码如下：




    const int maxEnemies = 3;
    const float maxEnemyHeight = 0.1f;
    const float minEnemyHeight = 0.5f;
    const float maxEnemyVelocity = 5.0f;
    const float minEnemyVelocity = 1.0f;
    Random random = new Random();
    GameObject[] enemies;





似乎有很多不明所以的东西，来看看他们都是些什么。






  1. 最大的敌人数，3个，好吧，我一定改了你。


  2. 最大敌人高度，0.1f，似乎可以想到。


  3. 最小敌人高度，0.5f，对啊，正如之前说到的，坐标系的Y轴是越上方越小。


  4. 最大敌人速率，5.0f，恩，敌人的速率是可变的，不错。


  5. 最小敌人速率，1.0f，最小的速度。


  6. 随机对象，一个Random对象，看来是创建随机数用的。


  7. 敌人数组，一个敌人的数组，和子弹一样，只是名字不同而已吧。




这就是我们第一步完成的事情，之前说过，微软工程师都是强大的存在，可以预见未来，他们可以在刚刚开始写代码的时候就声明好所有将来需要使用的变量。当然，这是个笑话。




之后要初始化这些对象，载入enemy.tga资源。找到LoadContent的位置，在初始化cannonballs的循环下面添加一个for循环。记得之前怎么做的吗？如法炮制。




    enemies = new GameObject[maxEnemies];
    for (int i = 0; i < maxEnemies; i++)
    {
        enemies[i] = new GameObject(
        Content.Load<Texture2D>(@"Spriteenemy"));
    }





不麻烦吧，只是吧cannnonballs的名字换一下就是这里的代码了。继续。




* * *







**第二步、更新UFO位置并杀掉屏幕以外的**




第二步做的是更新他们的位置，因为也是GameObject类，所以方法和cannonball一模一样。在Update方法中找到UpdateCannonBalls();这一行，在下面加入：




    UpdateEnemies();




说然后在类的其他任何地方加上我们的UpdateEnemies方法。




    public void UpdateEnemies()
    {
        foreach (GameObject enemy in enemies)
        {
            if (enemy.alive)
            {
                enemy.position += enemy.velocity;
                if(!viewportRect.Contains(new Point(
                    (int)enemy.position.X,
                    (int)enemy.position.Y)))
                {
                    enemy.alive = false;
                    continue;
                }
            }
        }
    }





真的，和CannonBalls一模一样，改一下名称，采用一样的杀掉的方法，这就是UFO移动，位置更新的代码了，目前为止没什么新的东西。




* * *







**第三步、新生UFO**




和cannonballs不同，在做cannonballs的工作的时候，先让它射击出来，也就是在按键的一刻，之后在离开屏幕的一刻把它杀掉。而enemies在被处理的时候，是先在他离开屏幕的时候杀掉，可是它从没出现过。所以这一步，我们要让他出现。




要新生一个enemy的GameObject我们要给他一些属性，包括alive要变成true，这是肯定的。然后要给他一个初始的position，一个velocity速度向量，velocity包括了方向，和速率。（所以我始终觉得speed更好）。




然后就是决定，什么时候创建它。它在离开屏幕的时候会死去，那么我们就在它死去之后马上把它救活，所以我们找到上一步的UpdateEnemies方法，在if(enemy.alive){}的后面，加上enemy.alive == false的代码，让它死掉马上活过来，并出现在屏幕的右方。




    if (enemy.alive)
    {
        // ...其他代码
    }
    else
    {
        enemy.alive = true;
        enemy.position = new Vector2(
        viewportRect.Right,
        MathHelper.Lerp(
            (float)viewportRect.Height * minEnemyHeight,
            (float)viewportRect.Height * maxEnemyHeight,
            (float)random.NextDouble()));
        enemy.velocity = new Vector2(
        MathHelper.Lerp(
            -minEnemyVelocity,
            -maxEnemyVelocity,
            (float)random.NextDouble()),
            0);
    }





看看我们做了什么。




enemy.alive = true; 没错，这是出生证明，起码在某些程度上说明了它活过来了。




enemy.position = new Vector2(); 这当然是给他位置向量，初始化向量要X坐标Y坐标，那X坐标就是屏幕右侧，X轴最大值，viewportRect记录了屏幕大小，调用viewportRect的Right属性，就是我们要的屏幕宽度，也就是X轴最大值。而Y坐标，我们要随机产生一个高度，这样才有游戏的味道。首先，我们用random.NextDouble随机生成一个0到1之间的double类型的随机数，这就是我们得到随机数的方法。但是，它不是我们要的数字，我们需要一个高度差之间的，而不是0到1的。于是这里要讲一下MathHelper.Lerp方法，它需要3个参数，随机数的最小值，随机数的最大值，和随机数。我们使用这个方法，把我们随机产生的那个书限定到我们给的新范围里。其实做法很简单，相信大家都可以写出来，(最大值-最小值)×随机值+最小值，这得到了我们的这个数，不过XNA帮你省去了这些代码（真是细致的过分了 = =），直接帮助得到我们的数。




enemy.velocity = new Vector2()，一样的，也是产生一个新的向量。因为要他笔直的从右往左穿过屏幕，那Y方向没有速度，Y给个0就好了。X则是要随机在我们定下的范围里得到一个数字，方法和position的类似，只是要注意的是，因为我们逆X轴正方向飞行，所以X轴速度是负数。




至此，完成了UFO的全部的Update工作。




* * *







**第四步、绘制UFO**




嘿嘿，一样，最后我要画UFO。找到Draw方法，加入一个新的SpriteBatch.draw就可以了。因为UFO不会和加农炮碰撞，所以无论放在背景以上的什么层面都好，只是我觉得起码应该遮掉子弹吧～于是添加代码，又是和cannnonballs一样。




    foreach (GameObject enemy in enemies)
    {
        if (enemy.alive)
        {
            spriteBatch.Draw(
            enemy.sprite,
            enemy.position,
            Color.White);
        }
    }





We're done here. 运行一下看看效果。（我修改了同屏敌人数，因为我的子弹改成是10 = = 并且我发现我的子弹是可以打成一张网的，这游戏没意思了）




[![](/images/uploads/zb/2009-06-16_Chapter7Runtime.jpg)](/images/uploads/zb/2009-06-16_Chapter7Runtime.jpg)




画面不错。OK，下一次就是要真的用子弹杀掉敌人了～







【官方源代码下载】




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=154)







* * *







**第三步的补充内容翻译。**




这段文字详细讲解了Lerp方法的成因，作用，实现方式，用法，好处等等，可以说非常的详细。




但我个人其实非常不理解，首先我非常不理解为什么（最大值-最小值）×随机值+最小值 这样一个计算就可以完成的事情一定要封装到framework里，虽然那个类的名字叫MaxHelper。另外不理解的地方就是为啥还要用这么多的文字来说明，看了就知道，它用了很多文字来说为什么0到1之间的数可以转化为一个范围内的数，还打了从一个数轴映射到另一个数轴的例子。




只是觉得很有意思而已，美国人做事情有些德国人的味道。看文字吧。




>

>
> **随机数和线性插入**
>
>

>
> 在游戏中，随机事件会添加更多的曲折。从随机出现在随机位置的玩家或敌人，到一定几率出现的奖励道具，随机事件可以保持一个游戏的挑战性和乐趣。
>
>

>
> 怎么才能让你使用一个随机数字来创建随机事件呢？
>
>

>
> Microsoft® Visual C# 2005 Express Edition和Visual Studio 2005的包含的标准库中提供了一个实用的类----System.Random类。它可以被实例化，也可以生成特定最大范围内的随机整数，或随机生成0到1之间的双精度小数。
>
>

>
> 乍看之下，产生一个随机证书似乎更管用。在某些情况下，它很管用。举个例子，随机访问你的数组索引，你需要一个整数。但是2D游戏中的数是浮点数，这意味着，他们拥有小数部分。我们需要一些不是严格的整数的数值。打个比方，5到10之间产生一个随机速率。如果我们只是用5到10之间的整数，只有6中可能的数值，这使得速率间的差异性看上去不是那么随机。
>
>

>
> 一个双精度浮点数之给我们更多范围内数据的变化，使可以转化为更多各种各样的表现。这非常美妙，特别是当我们屏幕上有很多对象拥有不同的位置和速度的时候。
>
>

>
> 然而，我们的Random类只返回0到1之间的双精度浮点值，或称为double类型的值，却对我们要随机获得5到10之间的数字不怎么实用。你当然可以做一些必要的乘法和加法来让0到1之间的数适应5到10之间这个范围，但是XNA Framework提供了更加简单的方法 ---- MathHelper.Lerp。
>
>

>
> Lerp是linear interpolation（线性插入）的缩写，一个数学方法，提供它一些0到1之间的数值T，并提供给它最大值和最小值，它可以计算出一个最大值和最小值之间的输出T。
>
>

>
> ![](/images/uploads/zb/BG_3.7.3.1pd.png)
>
>

>
> 把它想象成是在一个最小值和最大值之间的数轴上决定位置，而T是从开始的0到1之间的一条数轴上的某个位置。如果T是0.75，它应该在数轴范围的3/4的位置，在5到10的数轴上就是8.75。照着这个方法，一个0到1之间的数值可以用来在两个浮点数之间找到一个对应的浮点数。
>
>

>
> Lerp方法可以用在游戏中需要以恒定速率增加或减少的任何东西。速率、动画、淡出效果或变换颜色 ---- 简简单单传入一个0到1之间的数值T，一个最小值，一个最大值，Lerp会完成之后的所有事情。
>
>




