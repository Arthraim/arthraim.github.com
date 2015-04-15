---
layout: post
title: "未找到应用程序的“aps-environment”的权利字符串"
date: 2013-07-31 08:54
comments: true
categories:
- Programing
tags:
- iOS
- push
- Apple
---

总是想到这个忘了那个，记录一笔

1. bundle_id 和实际的 appid 一致
2. 选择的Provisioning profile对不对（有时候auto会使用通配的profile）
3. portal中对应的appid是否开启推送，是否有推送证书
4. provisioning profile是否是在appid开启推送后再生成的
5. 设备是否是adhoc的profile包括的设备
