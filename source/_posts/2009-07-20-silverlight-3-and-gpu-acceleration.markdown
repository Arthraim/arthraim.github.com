---
comments: true
date: 2009-07-20 21:27:24
layout: post
slug: silverlight-3-and-gpu-acceleration
title: Silverlight 3 和 GPU Acceleration
wordpress_id: 82
categories:
- Programing
tags:
- .net
- GPU
- Microsoft
- Silverlight
---




Silverlight3追加了很多新特性，其中不乏其特别重视的媒体支持，还有一些3D的小特性，以及这里写的GPU加速。我没有找到十足的佐证来证明打开关闭GPU加速之后究竟应用的效率发生了多少变化，事实上，这个变化也的确取决于CPU和GPU的性能，而在我有限的尝试中，没有找出非常有力的证据来证明打开GPU加速之后提高了多少效率。




结束掉XNA的教程，可以随便说说别的东西了，这样很有感觉。最近因为Google Reader的更新，通过新的Like功能follow了很多口味相似的朋友，更多的分享让我看到了更多感兴趣的东西。这次写Silverlight3里的GPU加速新特性，网上查了点资料想看看是怎样的，结果七零八落的，所以写一个吧。




**一、编码方法**




开启GPU加速的方法其实很简单，只需要在Object标签里设置一个属性就可以实现这一效果了，另外一些例子中提到了其他的一个辅助的属性，这里用最简单的例子讲一下。




**1、写UI**




UI元素我们简单一些，用MediaElement来播放一段视频。起初用了wmv，也可以顺利的说明这一问题，不过后来想到不如用H.264看看，也算是3的新特性了，放在一起用用，就找了个该编码的mp4视频，片子稍微有点老。




    <Canvas x:Name="LayoutRoot" Background="White">
    <TextBlock Width="800" Height="25"
        Canvas.Top="10" Canvas.Left="10"
        Text="Arthraim.cn GPU Acceleration Demo"
        FontSize="14"/>
    <MediaElement Width="640" Height="272"
        Canvas.Top="40" Canvas.Left="180"
        Source="http://localhost/H264_example.mp4"
        CacheMode="BitmapCache">
    </MediaElement>
    <Image Width="183" Height="272"
        Canvas.Top="40" Canvas.Left="10"
        Source="http://localhost/poster.jpg"
        Opacity="70">
        <Image.Projection>
            <PlaneProjection RotationY="-30" GlobalOffsetX="0"/>
        </Image.Projection>
    </Image>
    </Canvas>




编译运行一下，可以看到截图如下，正常的播放着。背景是加上了白色属性的，默认的也是白色，不过不加上这个属性，背景的处理效果就不一样了，可以试试看。




[![](/images/uploads/zb/2009-07-20_GPUacceleration1.png)](/images/uploads/zb/2009-07-20_GPUacceleration1.png)







**2、修改客户端网页**




然后真正来看看如何使用GPU加速。要做的非常简单，找到使用silverlight的客户端页面，添加下面的变量。




    <param name="EnableGPUAcceleration" value="true" />




这样还没完，光这样我们不知道究竟做了什么有价值的事情，在微软的帮助下，我们可以使用另一个属性 ---- enableCacheVisualization，这名字就取得很好，意思很明了。至于编码上也是一样，加上了变量，设置为true：




    <param name="EnableCacheVisualization" value="true" />




然后编译运行，哇，真是惨不忍睹，好好的视频怎么变成红色了，包括背景和左边的海报都一并红色了。其实这就是添加第二个属性的作用，红色的区域就是指没有应用GPU硬件加速的区域。




[![](/images/uploads/zb/2009-07-20_GPUacceleration2.png)](/images/uploads/zb/2009-07-20_GPUacceleration2.png)







**3、修改UI**




要使得硬件加速应用在对象上，那么最后还要修改UI的MediaElement。Silverlight3在UIElement中添加了一个public属性，名为CacheMode，以配合GPU加速的使用。而MediaElement 当然是可以华丽丽的使用CacheMode这个属性的。给刚才的代码修改一下。




    <MediaElement Width="640" Height="272"
        Canvas.Top="40" Canvas.Left="180"
        Source="http://localhost/H264_example.mp4"
        CacheMode="BitmapCache">




编译运行。中间没有红色遮盖的部分，就是真正表示了被GPU加速处理的部分。




[![](/images/uploads/zb/2009-07-20_GPUacceleration3.png)](/images/uploads/zb/2009-07-20_GPUacceleration3.png)







值得一提的是旁边的图片（本来是做对比用的，结果恰好遇到了这种情况）变成了绿色（无论Image是否设置了CacheMode属性），我Google了会儿，在官方论坛上也没有人答出个所以然来，据说没有加速的会被tint a color，但是没说绿色和红色分别代表了什么。我是先看到这个帖子，才自己碰到这个情况的，很巧这个例子刚好可以碰到，究竟绿色是怎么回事，知道的朋友告诉我下～




* * *







**二、简单性能测试**




我没有编写什么代码来做什么性能测试，这里只是简单的看看CPU占用率而已，想想GPU处理去了CPU总可以减负吧，但是结果应当说相当相当的不明显，不深究了。图左边的CPU使用情况那个数值是没什么作用的，那只是一个偶然的情况，我觉得看下来两者的确是有差异的，没加速的时候大部分时间看到的数字是50以上，而打开则在50左右跳动。旁边的波形图才是比较客观的反应的，放视频的几分钟监控到的情况。其中没有打开加速的时候出现一个大的波峰，是我尝试的切换最大化的IE窗口到VS08的窗口的时候发生的情况。在打开加速之后，我也切换了一次，也感到有一些卡，不过波形上表现并不十分明显。




另外，也不知这个所谓的GPU加速功能有没有用到CUBA技术。




[![](/images/uploads/zb/2009-07-20_GPUacceleration5.jpg)](/images/uploads/zb/2009-07-20_GPUacceleration5.jpg)




* * *







**三、附加3D测试**




无聊试试看同一个例子下，把MediaElement 给3D处理后还能不能硬件加速，绕Y轴旋转了点角度。从红色分布的情况来看似乎是不能加速，希望有朋友能继续探究一下这个问题。




[![](/images/uploads/zb/2009-07-20_GPUacceleration4.png)](/images/uploads/zb/2009-07-20_GPUacceleration4.png)







简单的写到这里吧，还有很多值得深入下去，当然，关于硬件加速方面留给微软的工作恐怕也还有很多很多。这个例子很好，不但遇到了一些特殊情况，还包括了H.264和3D等新特性的应用。
