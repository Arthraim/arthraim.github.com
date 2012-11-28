---
comments: true
date: 2009-07-13 17:26:58
layout: post
slug: xna-tutorial-3d-ch9-add-audio
title: XNA教程-3D游戏-08-添加声效
wordpress_id: 78
categories:
- Programing
tags:
- 3D
- XNA
- 游戏开发
- 教程
---







这个3D教程中，有一个对于2D也通用的，并且在之前的教程没有涉及到过的内容，就是音效，也就是这一次要完成的内容。用到了XACT来处理一些东西，那么教程就是音效的添加了，发射导弹之后的音效。当然作为XNA游戏，你也可以调用其他API来完成这个工作，不过这里讲的是标准的利用XNA的XACT来完成的步骤。




放假就是放假，感觉做正经事情的效率还真是很低，更不用说好好打理博客了。拖了几天，因为组织了一场初中同学会，可谓轰轰烈烈，难得可以请到那么多以前的老师，都很是感激和感动。言归正传，写程序吧。




**第一步、简介**




游戏很重要的一个因素是声音，在之前的2D教程中，却没有涉及到这个内容。在XNA中，我们使用的音效将是被包含在另一个声音工程当中的，这就要使用到XNA Game Studio中的一个工具 ---- Microsoft Cross-Platform Audio Creation Tool 简称为XACT。在开始菜单中选择它，有v2或v3两个版本，这里我们和教程一样v3，大概的功能都是一样的。（参见文章后方的翻译1、什么是XACT？）




![](/images/uploads/zb/2009-07-13_XACTs.jpg)




打开后界面如下图所示，除了工具栏，左侧上方是一个树形视图，有一些项目已经存在。




[![](/images/uploads/zb/2009-07-13_XactUserInterface.jpg)](/images/uploads/zb/2009-07-13_XactUserInterface.jpg)




* * *







**第二步、新建XACT工程**




**1、创建新工程**




类似Visual Studio的思路：选择File->New Project。




>

>
> ...TChapter8ContentAudioTAudio.xap
>
>





选择路径（比如我使用如上路径），保存为任意的名字，[确定]，这样创建就算成功了。




![](/images/uploads/zb/2009-07-13_NewXactProject.jpg)




**2、新建Wave Band**




然后在左边树形图上，WaveBand处右键->New Wave Band。新建一个Wave Band。




**3、新建Sound Band**




最后在左边树形图上，SoundBand处右键->New Sound Band。新建一个Sound Band。




[![](/images/uploads/zb/2009-07-13_NewWaveBand_NewSoundBand.jpg)](/images/uploads/zb/2009-07-13_NewWaveBand_NewSoundBand.jpg)




* * *







**第三步、添加Wave并创建Sound**




**1、插入wav文件**




右键单击右侧Wave Band的子窗口空白处，选择Insert Wave Files。




[![](/images/uploads/zb/2009-07-13_AddWavesToWaveBand.jpg)](/images/uploads/zb/2009-07-13_AddWavesToWaveBand.jpg)




记得我们之前排除到GS游戏工程外面的那两个wav文件吗，现在把他们找到并且插入进来。一个是发射导弹的声音，一个是爆炸的声音。




[![](/images/uploads/zb/2009-07-13_Waves.jpg)](/images/uploads/zb/2009-07-13_Waves.jpg)




插入之后如下图所显示的，这样我们准备好了Wave了。




[![](/images/uploads/zb/2009-07-13_AfterAddedWaves.jpg)](/images/uploads/zb/2009-07-13_AfterAddedWaves.jpg)







**2、创建Sound**




这里利用一些XACT提供的便捷操作，我们很容易办到。简单的把Wave Band子窗体内的两个wav，拖拽到Sound Band子窗体的Cue区域内，这样会自动创建出我们需要的一切。




[![](/images/uploads/zb/2009-07-13_AddWavesToCue.jpg)](/images/uploads/zb/2009-07-13_AddWavesToCue.jpg)




Wave, Sound, Cue是XACT中涉及的重要的概念，如上图我们是这样的处理顺序，但是其实内部处理时不是这样的。（参见文章后方的翻译2、设计时与运行时的XACT对象）




准备就绪后，保存并且退出，之后的工作，就是在GS里完成了。




* * *







**第四步、在GS工程中添加XACT工程**




完整的创建了一个XACT工程之后，我们就直接可以把它当作asset放进GS工程的Content中，并且在游戏中，直接可以从Content pipline去Load这些声音了。当然首先我们要把它添加进来。




右键单击Content下的Audio目录，选择Add->Existing Items。




[![](/images/uploads/zb/2009-07-13_AddXactProjectToGs.jpg)](/images/uploads/zb/2009-07-13_AddXactProjectToGs.jpg)




找到保存起来的xap工程文件，[确定]。（比如我刚才保存的是TAudio.xap，找到它）




![](/images/uploads/zb/2009-07-13_AfterAddedXactProject.jpg)




如上图，声音就添加成功了。




* * *







**第五步、加载声音内容**




之前我们载入一些资源都是使用了Content.Load这个方法，那么这里载入Audio将会有些不同，我们需要一些其他的方法来调用它们。




**1、声明 **




首先在Game类声明一些需要用到的私有变量。





    AudioEngine audioEngine;
    SoundBank soundBank;
    WaveBank waveBank;




没错，这里用到了三个类，两个很熟悉，SoundBank和WaveBank，应该是和之前的对应起来的。AudioEngine顾名思义，是处理声音的引擎，究竟如何工作，看下去。




**2、加载**




加载Content中的声音文件，是一个非常特殊的步骤，代码只有三行，在LoadContent方法中，SpriteBatch那一行的下面加上这样三行代码。





    audioEngine = new AudioEngine(@"ContentAudioTAudio.xgs");
    waveBank = new WaveBank(audioEngine, @"ContentAudioWave Band.xwb");
    soundBank = new SoundBank(audioEngine, @"ContentAudioSound Band.xsb");




注意，这三行代码的特殊之处，就在于三个后缀。




audioEngine中，不是使用了TAudio.xap这个文件，而是使用了TAudio.xgs这样的方式。如果看过之前对Content Pipline的介绍，那么你就不会不知道，Content不是简单的一个文件夹把文件组织起来，而是会对输入asset做处理并且有相应的输出，而这里的xap作为输入，我们是无法直接获得相应的Wave Band和Sound Band的。"xgs"缩写自XACT Global Settings，它是一个告诉你XACT工程如何组织文件的文件，Content Pipline只有依靠他才能找到我们需要的各种资源。大致说来就是这样。




一样的WaveBand和SoundBand的构造方法中，传入的是Wave Band.xwb（代表XACT Wave Band）和Sound Band.xsb（代表XACT Sound Band），而文件名则是我们之前在XACT工程中保留默认命名的Wave Band和Sound Band，当然你可以取不一样的名字。xwb和xsb类似xgs，也是输出的结果，就像之前的texture和model我们根本不使用后缀一样。




如果你编译了你的工程之后，可以在Debug目录下相应的位置找到这三个文件。（参见文章后方的翻译3、加载一个XACT工程）




* * *







**第六步、添加声音播放的代码**




**1、Update声音**




我们肯定要求声音跟着游戏循环播放，而不是在一帧里面放完就结束。




在Update方法中，找到UpdateMissiles();这行调用，在其下方插入一行。





    audioEngine.Update();




这个方法可以保证声音一帧一帧的更新并且播放，其实游戏中我们播放的既不是Sound也不是Wave，而只是Cue。




**2、 播放导弹发射声音**




在发射导弹的时刻播放一个missilelaunch的声音，下面来找到FireMissile()方法，在if (!missile.alive)之后插入这样一行代码。





    soundBank.PlayCue("missilelaunch");




编译运行，我们要的生效就出现了～










【官方代码下载】




[![](/images/uploads/zb/2009-06-12_download_XNA.png)](http://creators.xna.com/downloads/?id=163)




* * *







以下是一些XACT相关内容的翻译，总共有三个部分，分别对应第一步、第三步、和第五步的相关操作。




在翻译中涉及到了一些概念，其中经常被提到的有3个词，在原文中采用了小写，因此不是什么特有名字，但是却很难翻译成中文，这里特别提出一下。分别是wave,sound,cue这三个词。这是XACT涉及到的三个很重要的概念，它们环环相扣，所以翻译也许十分恼人。我把它们翻译成"波"、"声音"、"索引"，虽然不太贴切，但是可以保证在以下的文字中，只要出现这三个中文词，绝对就代表了这三个英文的原文单词。并且每一段文字中第一次出现这三个词的时候，都会在括号里写上相应的原文。另外设计到的一些首字母大写的专有名词都没有翻译。




（以下文字有本人翻译自XNA Creators Club官网，未经允许严禁转载；因本人水平有限，难免出错，望得到更正）




**1、什么是XACT？**




>

>
> 教程的这一部分将介绍包含在XNA Game Sudio中的一个新的程序----Microsoft Cross-Platform Audio Creation Tool，或者简称为XACT。
>
>

>
> 就像它的名字一样，XACT是一个跨平台的工具，制作音频输出用以Windows和Xbox360。XACT的设计允许音频独立于将要使用的平台来单独设计。
>
>

>
> XACT由两个主要部分组成：音频设计器（在这章节教程开始时你用到的图形化工具）和音频API（教程结束部分你会用到的代码）。音频设计器旨在允许任何一个为游戏设计音频的设计者可以进行操作，即便其不熟悉音频代码。
>
>

>
> 音频设计者使用所有原生的波形音频文件（.wav文件，被称为波（wave））。然后设计者组织这些波到声音（sound）形式，可以被让一个或多个波在各种时间和各种设定下被播放。最后，音频设计者组织声音到索引（Cue）形式。索引代表了在代码阶段可以访问的对象，并且相比实际的音频文件，更加接近游戏事件。
>
>

>
> 索引是暴露给程序员的，在合适的游戏事件发生时，使用XACT API调用索引播放。音频设计者同时可以设计一些运行时的参数，来修改播放的声音的音量或说是波幅，传递这些参数给程序员，使其能转化这些参数为游戏代码中的变量。
>
>

>
> 虽然起初看起来似乎有些复杂，但其优势可以在音频设计者要替换波形音频文件的时候，调整音频如何播放的时候，修改索引如何播放一个声音的时候，或改变设定的参数如何修改播放的声音的时候。音频设计者可以做所有的这些工作，而不必接触游戏源代码。
>
>

>
> XACT是一个快捷且灵活的工具，来添加健壮、饱满的音频到使用XNA Game Studio创建的游戏中。
>
>








**2、设计时和运行时的XACT对象**




>

>
> XACT工程有很多的组成部分。虽然其中一些部分是可选的，但一个基本形式的XACT对象对于XACT工程组织音频到XACT API还是非常必要的。
>
>

>
> 每个XACT工程包含了一个或多个Wave Bank。一个Wave Bank是一些波形音频文件的集合，称为波（wave）。这些Wave Bank中的波不是由XACT API直接访问的。而是给音频设计者转换其成为声音（sound）的。
>
>

>
> 这些声音组织成为一个或多个Sound Bank。一个Sound Bank是一些声音的集合。声音是使用各种参数去播放一个或多个波的时候的须知（instruction），这些参数可以是控制循环，变换效果，波的随机选择，或者更多其它的。
>
>

>
> Sound Bank也同样可以包含索引（cue）。索引是播放声音的须知（instruction to play sounds）。索引调用特定的声音，或随机选择声音，这取决于音频设计者如何设定这个索引。
>
>

>
> 当一个XACT工程已经建立，sound bank和wave bank就被编译到文件中了，一起的还有一个全局设定文件（global setting file）包含了一些工程特定的信息。这些文件在运行时使用XNA Framewrok来调用。
>
>

>
> 在sound bank、wave bank和全局设定文件被加载到游戏中后，程序员可以调用SoundBank.PlayCue，传入音频设计者已经定义好的索引的名字，来播放任意一个sound bank中索引。这样会按照音频设计者设计的默认方式播放这些索引，而没有任何程序员掌握的控制条件。
>
>

>
> 要得到播放的索引更多颗粒化的操作，包括暂停、回复和停止的功能，程序员可以调用SoundBank.GetCue并且得到一个Cue对象。然后程序员可以根据需要调用Cue.Play，Cue.Stop，CuePause，和Cue.Resume。然而，这样使用Cue对象仍然需要谨慎一些。如果你创建的一个Cue对象在声明的时候存在范围的限制（比方说只声明在一个方法中），就不要把Cue对象存储到静态化存储空间中，就像类的数组或List集合，这些Cue对象会在程序溢出这个范围之后停止并释放。
>
>

>
> 在一个索引被封播放之后，XACT音频引擎的任务就完成了。这取决于索引和其相关联的声音的关系，以及声音和其相关联的波的关系，然后按照音频设计者设计的那样来播放相应声音和波。
>
>








**3、加载一个XACT工程**




>

>
> 在XNA Game Studio游戏中播放音频需要加载XACT工程到XNA Framework内容管道（Content Pipline）中，编写加载XACT运行时输出文件所必要的代码，然后在游戏编译进程中XACT工程会被编译成为XACT输出文件。
>
>

>
> 音频内容和XNA Game Studio中大部分其他内容的形式不同。XACT处理生成的内容不像模型、贴图纹理或者字体那样在运行时使用ContentManager.Load方法加载。
>
>

>
> 取而代之的，是构造AudioEngine、SoundBank、和WaveBank这些XNA Framework中音频相关的类，用来加载编译后的音频文件。用正确的顺序加载这些文件，用名称和后缀定义它们都非常重要。否则，你可能会遇到错误。
>
>

>
> 要明白如何加载这些文件，那明白XACT工程编译后生成了什么文件就显得十分重要。
>
>

>
> XACT生成的第一个文件就是XACT工程文件（.xap）。这个文件是音频设计者在用音频设计工具保存音频工程时创建的。这个文件应该放置到Solution Explorer中，即把它添加到XNA Framework内容管道（Content Pipeline）的文件列表中去编译。
>
>

>
> 在编译开始后，内容管道把XACT工程文件编译成为一系列XACT输出文件。第一个生成的文件是全局设定文件（Global Settings File），默认命名为XACT工程文件一样的名字，扩展名为.xgs。如果XACT工程文件的名字是MyGameAudio.xap，那全局设定文件的默认的名字就是MyGameAudio.xgs。
>
>

>
> 编译过程之后会生成一个或多个wave bank文件，数量取决于音频设计者在XACT工程中添加的wave bank的数量。这些wave bank文件有一个后缀.xwb，并且由音频设计者在之前命名。默认的一个wave bank的输出文件名为Wave Bank.xwb。
>
>

>
> 编译过程还会生成一些sound bank文件，同样取决于音频设计者在XACT工程中添加的sound bank的数量。这些Sound bank文件有一个后缀.xsb，并且由音频设计者在之前命名。默认的一个sound bank的输出文件名为Sound Bank.xsb。
>
>

>
> 加载这些文件是初始化当使用XNA Framework播放音频时需要的代码对象的重要部分。加载工作表现为一步一步构造所有相关联的这些代码对象。全局设定文件用来构造AudioEngine。AudioEngine对象和wave bank文件同时使用，可以初始化一个WaveBank对象，而和sound bank文件一起使用，可以初始化一个SoundBank对象。
>
>

>
> 下面的插图解释了文件的一些关系，从音频设计者在设计时的形式，到通过内容管道编译时的形式，最终成为运行时游戏开始后被载入的形式。
>
>

>
> [![](/images/uploads/zb/BG_4.8.5.4pd.png)](/images/uploads/zb/BG_4.8.5.4pd.png)
>
>




