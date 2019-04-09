---
layout: post
slug: swift-5-string-native-encoding-change
title: Swift 5字符串内部编码变化
date: 2019-04-08 23:59:00
comments: true
categories:
- Programing
tags:
- Swift
- Swift5
- String
- UTF-8
- UTF-16
---

> TLDR：
> 1. 不要假设`String`实例里的编码是UTF-16 / UTF-8
> 2. 不要把`Range`实例和相关联的`String`实例分开使用

**这篇文章假设你已经知道Swift `Range<String.Index>` 对emoji或其他ligature的处理，和Objective-C `NSRange`是不同的！这边文章不是探讨这个问题的。**

下面详细说说来龙去脉，试着先说清楚我们遇到的实际问题，如果你没兴趣了解太多实际问题的上下文，可以直接跳到下文「实验」部分。

## 遇到的bug

遇到一个Crash，我们有一个功能，是在TextView删去一段字符（比如`@Arthur `）的最后一个字符（空格）的时候，删去整段字符（`@Arthur `），类似微信里你删除一个@的人的名字的时候的体验。

我实现的方式是：

1. 在 `textView(_:shouldChangeTextIn:replacementText:)` 回调中，检查如果用户在尝试删除 `@Arthur` 中那个空格的时候，找到`@Arthur`的Range，把Range记录下来。
2. 之后这个Range会被传来传去，最后在另一个地方拿着这个Range去删除 `textView.attributedText.string` 里的对应字符。当然我可以确保这个过程中 `textView.attributedText.string` 并没有发生变化。

两者都会把Swift `Range<String.Index>`转为`NSRange`，但转的时候基于的String是相同内容的不同实例。以上是前提。

在Swift4.2以及之前的版本里，都没有问题。升级到Swift5之后发生了Crash，表现是步骤 1 和 2 中拿到的 `Range` 并不相同。比如 1 的时候找到的是 `{2, 6}`，到了 2 的操作时候变成了 `{6, 16}`，这都是 `NSRange(range, in: string)` 来转的结果，如前面所说这个string是内容相同的不同实例。

这里真正就是这个不同的实例，1里的string是一个在Swift初始化的 `String` 实例，而2里是`textView.attributedText.string`。

看到这里可能nb的你已经注意到了，前者是一个swift `String`，在5的内部编码（native encoding）被改成了UTF-8；而后者虽然在编译时是一个swift `String`类型，但`textView.attributedText.string`原先是一个 Foundation 的 `NSString`，所以他的内部编码是UTF-16。

Range是在UTF-8上找到的，再应用到UTF-16的字符串上自然是有问题了。（比如ASCII在前者是单个Byte，后者则两个Bytes等）

## 实验

得到这个结论后，写了个例子

```swift
import Foundation

let string1 = "汉字Let's try to reproduce this bug.."
let string2 = String(NSString(string: "汉字Let's try to reproduce this bug.. "))
let substring = "Let"
if let range1 = string1.range(of: substring, options: [], range: string1.startIndex..<string1.endIndex, locale: nil),
    let range2 = string2.range(of: substring, options: [], range: string2.startIndex..<string2.endIndex, locale: nil) {

    let nsRange1 = NSRange(range1, in: string1) // {2, 3}
    let nsRange2 = NSRange(range1, in: string2) // {6, 3}

//    let nsRange3 = NSRange(range2, in: string1)
//    let nsRange4 = NSRange(range2, in: string2)
}
```
这是一个运行时的问题，`string2`的内部编码仍是之前的UTF-16，但`string1`则完全是一个（Swift5改动后）UTF-8的字符串。

这就是为什么`string1`中找到的`range1`，在用`string2`转成`NSRange`的时候会错。

<small>
（有意思的是如果把注释打开，`nsRange3`那一行会在运行时报错`EXC_BAD_INSTRUCTION`，你根本无法把`range2`用`string1`转成`NSRange`？）
</small>

## 所以……

所以第一反应是为什么语言层或者Foundation不帮忙做这个转换，但仔细想想，其实没人敢保证String/NSString里是什么编码的，没法自动完成这个转换。虽然`NSString`默认是UTF-16，swift `String`默认是UTF-8，但运行时谁也说不好操作的字符串是默认编码的，我们的代码也不该在任何时候做这样的假设。

所以把开头那两条东西再说一遍。

1. 不要假设`String`实例里的编码是UTF-16/UTF-8；
2. 不要把`Range`实例和相关联的`String`实例分开使用。

当然有很多转换的方法，Swift 5还deprecate了一堆比如`encodedOffset`这样的方法，开发者必须明确知道自己操作的String是什么编码的，可以用`utf16Offset<S>(in: S)`之类的完成之前的工作。不过这不是本文的重点了，这里的转换可以另外展开写篇文章？

总之从此之后，针对`Range`/`NSRange`的操作，除了以前要注意的emoji/ligature的不同处理以外，还要更细致的处理操作的`String`/`NSString`的编码，他们不再是无脑UTF-16了。


