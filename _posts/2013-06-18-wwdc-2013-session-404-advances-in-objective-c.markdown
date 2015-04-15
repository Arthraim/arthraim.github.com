---
layout: post
title: "WWDC 2013: session 404 advances in objective-c"
date: 2013-06-18 10:27
comments: true
categories:
- Programing
tags:
- iOS
- Apple
- WWDC
- objective-c
---


看了几个WWDC的session，做了一些笔记，简单贴在博客上吧。

404这个session大概说了objective-c的一些变化，和去年比起来改动实在是不多。

### module

这个改动比较引人瞩目，简单说就是写#import的时候改成@import。#include是复制粘帖，#import是#define保护下复制粘帖，@import则用了一个结构去维护。

在工程设置里，把「Enable Modules (C and Objective-C)」设为YES，不用改代码就能自动生效。会提高编译和索引的速度。

有一句话很刺眼叫「not available for user frameworks」，呵呵。

另外随之而来的Auto Linking特性，你不用自己去加那些依赖了。

### refactor tools

重构工具什么的，然后又提了提literals的语法什么的。说起后者今天看到[一篇文章](http://nshipster.com/object-subscripting/)，把object subscripting（这货就是比如说array[@3], dictionary[@"key"]这样）扩展成一门DSL。

### instancetype

这个改动我觉得挺重要。以前方法（尤其init）返回一个指针的时候用`id`作为类型。编译器其实很难（几乎不能）对它的类型作出判断。新增这个instancetype来声明返回值类型，可以避免很多不安全的问题。

比如Foo的init方法返回一个NSArray，调用的时候写成`NSDictionary *dict = [[Foo alloc] init];`。如果init方法是`-(id)init`，那编译器啥也不知道。如果改写成`-(instancetype)init`，xcode会警告你这里类型有问题了。

### explicitly-typed enums

`cell.selectionStyle = UITableViewCellAccessoryCheckmark`这种不同枚举类型的赋值会得到一个警告了。另外xcode的自动完成也更智能了。别忘了用`NS_ENUM`这个宏来写枚举哟。

### runtime

有一些改动，比如新的tagged pointer，以及使用isa的警告等等。下一节还提到了一些retain环的警告等等等等。

### GC -> ARC

苹果不要GC了，都用ARC了，包括Xcode 5在内的一些工程都放弃GC了。

另外ARC还有些进化。`__weak`更快了，说是x2快。dubug的`autorelease`处理更接近release build了。

### CF bridge

CoreFoundation框架的桥也全都优化了，提到implicit bridging的时候全场掌声，本人level太低不理解什么意思。

笔记大概就是这些，感觉很多都没怎么听懂，还需要补课。
