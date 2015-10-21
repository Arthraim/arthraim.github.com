---
layout: post
slug: "1password-leaks-your-data"
title: "1Password 会泄露你的数据"
date: 2015-10-20 14:12:16
comments: true
categories:
- Internet
tags:
- '1Password'
- security
---

值此网易泄漏了用户资料的好日子，我给大家结结实实的宣传了一把 1Password，好几个朋友立马买了。然而…… 很巧合的今天就看到了[一篇文章](http://myers.io/2015/10/22/1password-leaks-your-data/)，发现 1Password 的漏洞。本来打算发朋友圈告诉大家这个问题的，不过越说越复杂，还是放在博客里吧。

### 漏洞

你把用户名和密码全都告诉 1P，那么理论上 1P 应该把这些数据「加密」存起来，那么无论你用 iCloud 还是 Dropbox 来同步这些密码，都不会造成泄漏。

但是，事实上 1P 把这些数据保存成了一个，.agilekeychain 文件（目录），而这个目录并不安全。.agilekeychain 里提供了一个 [1Password Anywhere](https://support.1password.com/guides/mac/1passwordanywhere.html) 的功能，简单来说就是这个文件本身是个可以在任何浏览器打开的完整的 1P 网页客户端，你在任何地方打开这个目录里的网页就可以查到自己的用户名和密码了。而这个网页客户端为了访问到你所有 1P 里的数据，居然放了一个明文的 js 文件。

**UPDATE**: 原来 1Password 官方也是有[回应](https://blog.agilebits.com/2015/10/19/when-a-leak-isnt-a-leak/)的

### 解决方法

1. 不要上传这个文件到互联网上，并自己保证自己设备的安全。设备间同步用 WIFI 同步。（但是用我同事的话说，这比一张纸的安全级别还要低，传播成本比纸张小多了。）
2. 1P 官方提供了一个新的存储格式 OPvault，尽可能使用这个格式。Mac 转换方法请看[这里](https://discussions.agilebits.com/discussion/39875/getting-your-data-into-the-opvault-format)，其他平台 1P 的 [Blog](https://blog.agilebits.com/2015/10/19/when-a-leak-isnt-a-leak/) 里有提到

不开心 >.< 早上我弟在和我说，保存密码不是技术问题，是道德问题。天下无贼、夜不闭户才是终极诉求……
