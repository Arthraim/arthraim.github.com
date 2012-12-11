---
comments: true
date: 2010-01-26 17:07:32
layout: post
slug: install-32bit-jre-on-64bit-ubuntu
title: 64位Ubuntu上安装32位JRE
wordpress_id: 137
categories:
- O.S.
tags:
- Java
- Linux
- Ubuntu
---







装64位操作系统是一件很折腾人的事情，因为一些支持不好的软件往往只是考虑32位，比如flex for Linux alpha。我只是想尝试着在Linux环境搭建以下Flex Builder，没想到遇到很多问题…… 好吧，本文正题是安装32位JRE。Google到[这篇文章](http://www.albertsong.com/read-167.html)，介绍了一个比较智能的办法～






* 去Sun的官方网站下载一个32位的JRE包。比如，jre-6u18-linux-i586.bin。


* 安装java-package

```sh
sudo apt-get install java-package
```





* 使用java-package将32位的jre做成一个.deb包

```sh
DEB_BUILD_GNU_TYPE=i486-linux-gnu DEB_BUILD_ARCH=i386 fakeroot make-jpkg jre-6u18-linux-i586.bin
```





* 安装.deb包（无视deb包的名字里的amd64字样）

```sh
sudo dpkg -i sun-j2re1.6_1.6.0+update18_amd64.deb
```





* 32位的JRE到这里已安装完成，位置在`/usr/lib/j2re1.6-sun`。

  可以使用以下命令切换JRE了～（可以运行flexbuilder_linux_install_a5_112409.bin了）

```sh
sudo update-alternatives --config java
```






小记一下～




其实还有个我原创的比较弱智的办法






  1. 直接运行jre-6u18-linux-i586.bin，会自解压


  2. 之后cd到jre-6u18-linux-i586/bin这个目录里的话可以发现可以运行的二进制文件java。


  3. 那么就改一下.bashrc文件，加上一行：


```sh
alias java='你的路径/jre-6u18-linux-i586/bin/java'
```



以上……
