---
layout: post
title: "WWDC 2013: session 203 What’s New in Cocoa Touch"
date: 2013-06-19 10:43
comments: true
categories:
- Programing
tags:
- iOS
- Apple
- WWDC
- objective-c
---


继续简单贴WWDC的笔记。这个session内容相当多啊。

session 203 关于Cocoa Touch的改动，先看这个也是个人兴趣所致。随着iOS7的大动作，Cocoa Touch的改动真心多，还有更多更多细节可能没发现。

### 多任务

iOS7中多任务是一个重头戏。这里讲到三种。这些改动都是让我非常激动的。

#### background fetching | 后台抓取

这个应该是为了满足类似ping、心跳这样的需求，定时往服务器去抓数据。xcode5的Capabilities设置里，打开Background Modes，可以直接勾选到一个background mode：「Background fetch」。

主要涉及的API如下

```objective-c
- (void)application:(UIApplication *)application performFetchWithCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler;
- (void)setMinimumBackgroundFetchInterval:(NSTimeInterval)minInterval;
```

#### remote notifications | 远程通知

这是个很酷的功能。让app能够在接到一条推送消息的时候后台打开一个下载任务。

例如微信收到新消息推送后打开app，还会有一个连接下载的过程。现在接到推送的同时，就可以在后台把新消息抓过来，打开应用后app已经呈现了这个消息。另外可以想到的更酷的应用，是例如追美剧的那些视频软件，美剧一更新就通过推送把新的一集抓下来，用户下次进入app的时候就可以直接离线看了。

同样Capabilities设置里有一个选项「Remote notifications」

```objective-c
- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler;
```

#### background transfers | 后台传输

这是个比较基本的功能，发起一个网络请求可以在app切到后台或关闭后继续完成（当然你也可以中端或暂停它）

这里有个变化是Cocoa提供了一个新的类NSURLSession来取代NSURLConnection，这个新类可以快速发起background的请求。请求完成后app可以处理一个回调，展示下载完成的图片或其他资源。更多关于NSURLSession的内容在WWDC session 705「What's new in Foundation Networking」中有更具体的说明。

```objective-c
- (void)application:(UIApplication *)application handleEventsForBackgroundURLSession:(NSString *)identifier completionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler;
```




### Views and Images

#### UIImage的renderingMode

`- (UIImage *)imageWithRenderingMode:(UIImageRenderingMode)renderingMode;` 可以得到一个设置了renderingMode的图片，默认他是auto的，另外它还会有original和template两种绘制模式，后者无视图片中的颜色信息。

#### Tint

现在好像可以很懒惰的处理整个app的风格了，只要设置好tint，各种文字、图标的颜色都会自动切换过去。具体涉及到`UIView`的`tintColor`属性以及`tintAdjustmentMode`属性。而新的代理`- (void)tintColorDidChange;`可以让你知道tint改变的情况。

#### UIView Animation

iOS7的动画加强了不少。涉及动画的改动也不少。

`+ (void)performWithoutAnimation:(void (^)(void))actionsWithoutAnimation;` 可以强制一些动作不使用动画。不知道之前让我郁闷的要死的`UITableViewRowAnimationNone`还是有动画的问题能不能解决。

CocoaTouch提供了关键帧动画的API。

```objective-c
+ (void)animateKeyframesWithDuration:(NSTimeInterval)duration delay:(NSTimeInterval)delay options:(UIViewKeyframeAnimationOptions)options animations:(void (^)(void))animations completion:(void (^)(BOOL finished))completion;
- (void)addKeyframeValue:(id)value time:(CGFloat)time;
```

另外还有Motion Effects，动画可以结合重力感应什么的大概是这样。




### Collection View

这个是去年的新东西，强大到什么瀑布流、coverflow都能用layout实现。今年提供了一个在layout之间快速切换的方法。数据不变，layout换一下界面从瀑布变coverflow不是梦想。具体细节不纠结了。




### View Controllers

其实我觉得最担心的就是这个东西。因为可以看到statusbar navigationbar tabbar都是半透明毛玻璃了，我怎么让一个tableview显示在整屏却只在中间区域让用户滚动呢（能滚到中间，不会有cell躲在tabbar下面，但要让tabbar半透明看到滚下去的cell）？

viewcontorller涉及到下面的一些改动。

#### wantsFullScreenLayout

现在的viewcontroller始终保持iOS6设置`wantsFullScreenLayout`为YES的状态。StatusBar、NavigationBar等等都会变成半透明。真是丧心病狂！然后`wantsFullScreenLayout`这个属性就没了。因为它被始终设为YES。

#### extended edges

这个extended edges的属性，帮我解决了之前的担心。现在怎么让一个scrollView内容显示在中间部分，却可以滚到那些Bar下面实现半透明，就靠这个属性。让viewcontroller的extended edges变成你要躲开的控件的高度宽度就可以。描述不清楚，你们体会下。orz

#### content size

用`preferredContentSize`属性来设置你期望的大小，iOS7现在会用它来决定自己的大小。一般应该只有子界面才用得到。

#### status bar

因为全屏，所以现在的statusbar是没有背景颜色的。 `UIStatusBarStyleBlackTranslucent`和`UIStatusBarStyleBlackOpaque`两个枚举都被deprecated了。新的`UIStatusBarStyleLightContent`可以让你设置，万恶的毛玻璃。`UIStatusBarStyleDefault`不变。

#### custom transition

可以定制transition。比如navigation的push pop，viewController的present dismiss，或者手势控制的各种。另外新的代理`UIViewControllerTransitioningDelegate`（viewcontroller有相应属性）和新的protocol`UIViewControllerAnimatedTransitioning`、`UIViewControllerInteractiveTransitioning`、`UIViewControllerContextTransitioning`，具体的方法文档有，不知道用到的机会有多少。




### 状态恢复

现在新的App状态恢复，系统会给你截一张图，让你的程序的恢复变得无缝。

但事实上，很有可能截图和应用恢复后实际的数据或界面不一样，于是你可以用新的方法`- (void)ignoreSnapshotOnNextApplicationLaunch;`屏蔽之前的截图。

另外你可以用`+ (void) registerObjectForStateRestoration:(id<UIStateRestoring>)object restorationIdentifier:(NSString *)restorationIdentifier;`把__和界面无关__的数据对象保存下来。

蓝牙状态也可以恢复，这一块不太熟悉，总之系统帮你保存了一些状态，有两个新的key让你取到他们。




### AirDrop

这个session里笼统的说了一下，你可以注册protocol，每个app沙箱多了一个inbox目录等等，不深入了（视频里也没深入说啊）。




### Dynamics

「Getting started with UIKit Dynamics」和「Advanced Techniques with UIKit Dynamics」两个session详细介绍了这一块。

具体这货就是让你做一些效果，两个view粘在一起，view带上物理特效，碰撞、重力感应什么的高级货。

 * `UIDynamicAnimator` 操作一些动画的模型
 * `UIDynamicBehavior` 记录各种动作的一棵树
 * `UIDynamicItem` 这个protocol让控件变成你要操作的对象

### Text

iOS7的设置里，用户可以用一个滑槽修改字体大小了。

开发者可以用`UIApplication`的`preferredContentSizeCategory`属性取到用户设置的字体大小。

取到的值是一个`UIContentSizeCategorySize...`枚举，有XS、S、M、L、XL、XXL、XXXL几个档次。

用户修改这个设置的时候，会发送一个notification: `UIContentSizeCategoryDidChangeNotification`，使用新的key`UIContentSizeCategoryNewValueKey`可以从notification中取到修改后的字体大小。

另外`+ (UIFont *)preferedFontForTextStyle:(NSString *)style;`方法可以用「标题文字」「正文文字」这样的形式来获得一些固定的UIFont。

### Text Kit

这是一个全新的Kit，基于CoreText。具体「Introducing Text Kit」和「Advanced Text Layouts and Effects with Text Kit」以及「Using fonts with Text Kit」三个session涉及到这一块。

这货大致有三个类

 * `NSTextContainer` 是最外面文字布局的容器
 * `NSLayoutManager` 是内部的布局
 * `NSTextStorage` 文字的数据

修改文字布局只要按需要派生其中一层就可以，还有个`NSTextAttachment`可以图文混排什么的。细节跳过。

### More

Session最后提到了其他一下CocoaTouch的修改，罗列一下。

 * Local networking
 * Sprite Kit
 * game controllers
 * MapKit
 * CoreLocation
 * game center
     * new turn based api (like letter press)
