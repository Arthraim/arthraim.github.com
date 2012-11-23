---
comments: true
date: 2009-07-19 23:40:59
layout: post
slug: xna-tutorial-3d-ch9-create-enemy-ufo
title: XNA教程-3D游戏-09-添加敌方UFO
wordpress_id: 79
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---




应当说之前的Chapter08是插进来的一个部分，让我们的工程还没有变得非常巨大之前，在我们第一个需要声音的对象创建之后，插进如何制作音频的教程。而按照正常的逻辑，我们现在应该添加敌人，让我们的导弹"有的放矢"（有地方使）。和2D类似，UFO是我们的敌人，他从屏幕的最远端向我们靠近，而最终飞过我们的头顶后消失，其实就游戏逻辑来说和2D是一回事，只是所在空间和视角不同了。




惯例，在第一段之后说一些废话。最近更新的非常缓慢，不当心看了看最近博客的点击量发现这数字也是小的可怕至极。不过我不太关注那些东西的，只是自己也期待着快点把这个教程结束，然后继续我原来的博客路线。本来以为暑假可以用XNA做点东西，所以觉得写这个也挺有趣，不过最近做别的东西中，所以这个教程快快结束吧，希望能帮助到有需要的人就对了。说起来2D教程只用了一个星期啊……




言归正传，开始这一节的教程吧。




**第一步、初始化UFO**




和terrain以及missilelauncher的base或head一样，载入一个GameObject的典型方法也同样适用于UFO，只是不止一个UFO，所以我们还是类似2D的，使用了GameObject的数组。




**1、声明**




在Game类，你声明了一大串一大串的地方添加如下的声明。



    
    Random r = new Random();
    const int numEnemyShips = 3;
    GameObject[] enemyShips;





	
  * r是一个Random对象，可以让我们用来随机确定UFO的初始位置及速度。

	
  * numEnemyShips看来是一个会被我恶搞的变量，指同屏（最大）出现的UFO数量。

	
  * enemyShips当然就是GameObject的数组，数量由上一个变量确定。




**2、实例化**




找到LoadContent方法，在方法的最后加上实例化一个GameObject的代码。



    
    enemyShips = new GameObject[numEnemyShips];
    for (int i = 0; i < numEnemyShips; i++)
    {
        enemyShips[i] = new GameObject();
        enemyShips[i].model = Content.Load<Model>(
            @"Modelsenemy");
        enemyShips[i].scale = 0.1f;
        enemyShips[i].rotation = new Vector3(
            0.0f, MathHelper.Pi, 0.0f);
    }




实例化数组后，用一个 for 循环遍历这个数组。




每一次遍历，先实例化GameObject，然后确定模型以及缩放和旋转，还记得旋转吗？Pitch, Yaw, Roll，所以这里的初始化值是0,pi,0，yaw了180度，即朝向屏幕外的我们。




这里没有对其他的一些成员做初始化，它们被留到Update里面去处理。




* * *







**第二步、更新UFO**




留了一部分工作放在Update里完成，所以Update的工作也比较大，和Missiles一样，我们写另外一个方法来处理。




方法命名为：UpdateEnemyShips()。把它的调用放在Game类中Update方法处理音频update的后面。




然后写UpdateEnemyShips方法的内容如下：



    
    void UpdateEnemyShips()
    {
        foreach (GameObject ship in enemyShips)
        {
            if (ship.alive)
            {
                ship.position += ship.velocity;
                if (ship.position.Z > 500.0f)
                {
                    ship.alive = false;
                }
            }
            // next step...
        }
    }




因为已经实例化，所以可以用foreach来遍历了。如果UFO存活，那么位置加上速度即更新为当前位置。但是也必须处理如何杀掉它，记得第一段说的，"飞过头顶的时候"杀掉，那么就是检查Z轴就可以了。




注释部分是留给下一步，完成第一步欠下的，UFO的一些初始化工作。




* * *







**第三步、继续初始化UFO**




就像这一步的名字一样，继续初始化，这是第一步欠下的债。第一步没有个position和velocity初始化，因为所有的enemy的这两个属性不是一次性初始化就好了的，随着Update里的死亡，他们要重新被初始化，所以我们不把这个工作放在第一步的LoadContent方法里，而是放在Update方法里。




**1、声明**




同样的我们需要先声明几个变量，帮助我们使用随机数。在Game类内声明以下变量：



    
    Vector3 shipMinPosition = new Vector3(-2000.0f, 300.0f, -6000.0f);
    Vector3 shipMaxPosition = new Vector3(2000.0f, 800.0f, -4000.0f);
    const float shipMinVelocity = 5.0f;
    const float shipMaxVelocity = 10.0f;




如果你看过2D教程，那么这四个变量的用意就一定知道。




两个Vector3把起始位置限定在了一个立方体内。X轴-2000～2000（屏幕的左右位置），Y轴300～800（屏幕的上下位置，即高度），Z轴-6000～-4000（屏幕的深度，即距离屏幕6000到4000这个距离）。




而velocity，即速度，因为我们的UFO走平行于Y轴的直线，所以只用float限定他在Z轴上的速率。5到10之间。




**2、继续更新UFO**




第二步的时候，我们处理了或者的UFO如何的更新，和如何杀死。现在要处理杀死的UFO，如何复活，包括游戏刚刚运行时所有的都在死掉状态的UFO如何初始化。在第二步的方法注释部分添加如下代码：



    
    else
    {
        ship.alive = true;
        ship.position = new Vector3(
            MathHelper.Lerp(
                shipMinPosition.X,
                shipMaxPosition.X,
                (float)r.NextDouble()),
            MathHelper.Lerp(
                shipMinPosition.Y,
                shipMaxPosition.Y,
                (float)r.NextDouble()),
            MathHelper.Lerp(
                shipMinPosition.Z,
                shipMaxPosition.Z,
                (float)r.NextDouble()));
        ship.velocity = new Vector3(
            0.0f,
            0.0f,
            MathHelper.Lerp(
                shipMinVelocity,
                shipMaxVelocity,
                (float)r.NextDouble()));
    }




代码看起来很长，但其实逻辑很简单，我们只修改了三个属性：alive、position、velocity。




只是我们用到了MathHelper.Lerp方法和Random.NextDouble方法嵌在代码里。Lerp限定r.NextDouble生成的一个随机的双精度浮点数的范围，分到X,Y,Z三个分量上而已。而velocity就像之前说的，只需要Z轴有速度，所以只在Z轴用了相应的方法。




那么处理完这些，最后一步就是绘制UFO了。




* * *







**第四步、绘制UFO**




找到Draw方法，和missiles的画法一样，用一个foreach循环就搞定了。多亏了我们有写DrawGameObject这样的方法。



    
    foreach (GameObject enemyShip in enemyShips)
    {
        if (enemyShip.alive)
        {
            DrawGameObject(enemyShip);
        }
    }




好了好了，这样就没有问题了。编译、运行。（效果如图，点击放大）我承认我改了UFO数量。




[![](/upload/2009-07-19_Runtime.jpg)](/upload/2009-07-19_Runtime.jpg)




【官方源代码下载】




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=164)
