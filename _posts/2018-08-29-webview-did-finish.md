---
layout: post
slug: webview-did-finish
title: webView(_:didFinish:) 的时机
date: 2018-08-28 20:35:08
comments: true
categories:
- Programing
tags:
- Swift
- Swift4
- WKWebView
- iOS
---

更新一个简单的逻辑 -。-

客户端在用 `WKWebView` 的时候需要为前端页面加一些脚本，本来料想页面加载完成之前是不会用到这些脚本的（多半是用户的主动行为）所以选择在 `webView(_:didFinish:)` 回调里调用 `evaluateJavaScript(_:completionHandler:)` 来实现。

这次遇到一个页面，一加载完成（`DOMContentLoaded`）的时候就想有被注入的脚本的能力，于是不得不把注入的时机提前。`webView(_:didFinish:)` 的时机太晚了。

这就不得不用到了 `WKUserScript`，`atDocumentEnd` 是最符合前端要求和预期的时机。具体的时序是这样的（示例代码[在此](https://github.com/Arthraim/webViewLifeCycle)）：

```
webView:(_:didStartProvisionalNavigation:)
WKUserScript atDocumentStart
webView:(_:didCommit:)
DOMContentLoaded
WKUserScript atDocumentEnd
webView:(_:didFinish:)
```

看起来完美 -。-

另外一个坑是，如果 WebView load 一个本地的 html 文件，`webView:(_:didCommit:)` 会发生在 `WKUserScript atDocumentEnd` 之后，不懂怎么回事 O.O

