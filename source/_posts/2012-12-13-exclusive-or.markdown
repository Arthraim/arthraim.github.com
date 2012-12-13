---
layout: post
slug: exclusive-or
title: 异或
date: 2012-12-13 12:36
comments: true
categories:
- Programing
tags:
- xor
- ruby
---

不介绍什么是异或了，有人叫半加、数学系的叫按位模2加



下文用得到的一些简单的性质



* `x^0 = x` 且 `x^x = 0`

* 交换律：`x^y = y^x`

* 结合律：`(x^y)^z = x^(y^z)`

* 自反性：`x^y^y = x`


下面是几个小题目，可以用异或解决，挺有技巧性



## 交换两个数ab

```ruby
a = a^b
b = a^b
a = a^b
```

有意思的是搜索其他异或例子的时候，发现了[这篇](http://blog.chinaunix.net/uid-1844931-id-3034714.html)文章，文章里实现了一个异或交换的算法，和本文主题无关，不过很有意思，函数更多的时候应该只操作值而不是变量。





## A集合里拿掉数x得到B集合，求x

令`XOR(X)`表示将X集合内所有的数做异或

`XOR(B)^XOR(A) = XOR(B)^XOR(B)^x = 0^x = x`


```ruby
A = (1..10000).to_a
B = A - [1234]
x = (A + B).reduce(&:^)
puts x #1234
```

via: [http://www.javaeye.com/topic/420487](http://www.javaeye.com/topic/420487)





## A集合里拿掉数x、y得到B集合，求x和y


首先按上一个的办法可以推导出`xor(A)^xor(B) = xor(B)^xor(B)^x^y = 0^x^y = x^y`

`x^y`的二进制结果，第n位为1，说明x和y的第n位不相同


根据第n位是否为0把A里所有的数分成A1和A0两个数组（A1里的数的二进制第n位都是1，A0都是0）


A1和A0应该各包含了a或者b（这样第n位才能异或出1）


同理可以把B分成B1和B0两个数组


可以得到第一个数 `x = A1^B1`

第二个数可以`y = A0^B0`，当然也可以用`x^y^x`求得

另外如果`x^y`为0，即`x == y`，令`SUM(X)`为X集合内所有数求和

`(SUM(A) - SUM(B)) / 2 = x`

via: [http://blog.chinaunix.net/uid-12453618-id-2935334.html](http://blog.chinaunix.net/uid-12453618-id-2935334.html)





## 集合A里只有数x出现1次，其余数全都重复出现2次，求x

`xor(A) = x^y^y^…^z^z = x^(y^y^…^z^z) = x^0 = x`

```ruby
x = A.reduce(&:^)
```




## 集合A里只有数x出现1次，其余数全都重复出现3次，求x


xor的本质相当于“按位模2加”（adding modulo 2），令p1,p2…pn为布尔值，true为1、false为0，(+)表示异或操作。

`p1 (+) p2 (+) ... (+) pn == ( p1 + p2 + ... + pn ) % 2`

所以只需要实现按位模3加

`( p1 + p2 + ... + pn ) % 3`

将集合中所有数二进制表示的同一位的0或1相加，最终的和对3去摸，得到的数即是x

如 `A = {5, 7, 7, 7}`，二进制表示两个数

```ruby
  101
  111
  111
+ 111
------
  434
%   3
------
  101
```
但愿这坨东西能让人看懂

via: [http://www.cs.umd.edu/class/sum2003/cmsc311/Notes/BitOp/xor.html](http://www.cs.umd.edu/class/sum2003/cmsc311/Notes/BitOp/xor.html)





## 没有`^`操作时候实现异或，只用`&`与 `|`或 `~`非

这里的转换有很多，比如下面这个

`x ^ y == (~x & y) | (x & ~y)`

具体查看[维基百科](http://en.wikipedia.org/wiki/Exclusive_or) 《Equivalencies, elimination, and introduction》部分，各种公式

