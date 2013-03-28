---
layout: post
slug: uiwebview-load-local-javascript
title: "UIWebView加载本地资源"
date: 2013-03-27 18:19
comments: true
categories:
- Programing
tags:
- iOS
- objective-c
- javascript
---

作为一个不会javascript只会jQuery的程序员，我在做[Objective-c和HTML5页面交互的demo](https://github.com/Arthraim/objs)的时候需要HTML至少依赖一个jQuery，放在本地咋整？

##创建UIWebView的时候

```objective-c
_webView = [[UIWebView alloc] initWithFrame:CGRectMake(0,0,self.view.bounds.size.width,self.view.bounds.size.height)];
NSString *htmlFile = [[NSBundle mainBundle] pathForResource:@"index" ofType:@"html"];
NSString* htmlString = [NSString stringWithContentsOfFile:htmlFile encoding:NSUTF8StringEncoding error:nil];

NSString *path = [[NSBundle mainBundle] bundlePath];
NSURL *baseURL = [NSURL fileURLWithPath:path];

[_webView loadHTMLString:htmlString baseURL:baseURL];
// 把baseURL知道bundle的Url，就能调用bundle里的其他文件了，图片音乐什么的
[self.view addSubview:_webView];
[_webView release];
```

##添加.js文件的时候

添加js文件有点特殊，xcode会以为js是可以编译的代码。要在Xcode的target里，把.js文件从 "Compile Sources" 移到 "Copy Bundle Resources" .

##写HTML的时候

可以直接写相对路径了。`<script src="jquery.min.js"></script>`

**题外话1**: 有个基友要做一个HTML5播放视频的东东，想把视频缓存到本地，但是UIWebView无视HTML5 mainifest里指定的音频视频文件，还有5M的限制。其实也可以存下来本地加载撒。

**题外话2**: objc可以用stringByEvaluatingJavaScriptFromString:在webview里执行脚本。js要调用objective-c就比较麻烦。iOS可行的办法是利用custom URL scheme，跳转到应用之后应用或从path或从querystring来判断js要调用什么类什么方法，比较蛋疼。最理想的方式应该是[safari插件支持的方式](https://developer.apple.com/library/mac/#documentation/AppleApplications/Conceptual/SafariJSProgTopics/Tasks/ObjCFromJavaScript.html)，可惜iOS没有。

via:

* [https://devforums.apple.com/message/32282](https://devforums.apple.com/message/32282)
* [http://mentormate.com/blog/iphone-uiwebview-class-local-css-javascript-resources/](http://mentormate.com/blog/iphone-uiwebview-class-local-css-javascript-resources/)