---
comments: true
date: 2009-07-20 00:23:33
layout: post
slug: xna-tutorial-3d-ch10-destroy-ufo
title: XNA教程-3D游戏-10-摧毁UFO
wordpress_id: 80
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---




运行一下现在的样子，实在是已经相当满意了。一个terrain覆盖的世界，一个稳稳的launcher_base，一个可以在base上自由旋转的launcher_top，并且可以使用这个launcher发射无数的导弹，远处也会飞来无数的敌方UFO，看上去游戏已经非常完整了，唯独偏偏少了一点点----导弹无法射落UFO。那么这次的教程显而易见，就是用到3D碰撞检测，来让missile和enemyship来个大碰撞。




这次的教程不是很复杂，所以今天心情好就赶快把他结束掉，可以让博客转入让我自由发挥的方向。那马上开始吧～




**1、修改UpdateMissiles()**




先要修改一下这个方法，在if (missile.position.Z < -6000.0f) { missile.alive = false; } 后面加上一个else，然后调用一个莫须有的方法，代码如下：



    
    else
    {
        TestCollision(missile);
    }
    




当然这个方法还没有加上。




**2、写TestCollision()**




先把这个方法全部的代码贴上吧：



    
    void TestCollision(GameObject missile)
    {
        BoundingSphere missilesphere =
            missile.model.Meshes[0].BoundingSphere;
        missilesphere.Center = missile.position;
        missilesphere.Radius *= missile.scale;
        foreach (GameObject ship in enemyShips)
        {
            if (ship.alive)
            {
                BoundingSphere shipsphere =
                    ship.model.Meshes[0].BoundingSphere;
                shipsphere.Center = ship.position;
                shipsphere.Radius *= ship.scale;
                if (shipsphere.Intersects(missilesphere))
                {
                    soundBank.PlayCue("explosion");
                    missile.alive = false;
                    ship.alive = false;
                    break;
                }
            }
        }
    }
    




解释一下，BoundingSphere类是记录的是一个球形，也就是3D模型最大的外轮廓。用这个类的intersects方法和其他模型的boundingsphere确定是否相交，虽然粗糙，但是基本上已经符合我们的要求了。（相信我，即便是这样，依然难度很高）要确定BoundingSphere的中心点为模型所在的position，而半径要记得乘上scale，不然就不对了。




那么实际检验的地方就很简单，拿到一个missile后，遍历每一个UFO（两层循环），然后检查是否相交，如果相交就杀掉双方，并且播放声音----我们之前准备的爆炸的声音。




OK，编译运行，我们这个教程的最后一部分就完成了。（放效果图，不过放了也看不出碰撞）




[![](/upload/2009-07-20_Runtime.jpg)](/upload/2009-07-20_Runtime.jpg)




【官方源代码下载】




[![](/upload/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=165)
