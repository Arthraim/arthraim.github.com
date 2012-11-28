---
comments: true
date: 2009-04-19 00:09:13
layout: post
slug: 8-popular-browsers-standard-supporting
title: 8款主流浏览器对标准的支持程度横向测评
wordpress_id: 11
categories:
- internet
tags:
- Browser
- Chrome
- Firefox
- IE
- Maxthon
- Opera
- Safari
---

在上篇文章更新的时候觉得，不如重新写一篇，然后多下几个浏览器来试试看。于是这篇文章来了，总共选了8个我比较关注的或者大家比较常用的浏览器，先把今天的小白鼠来列一下。










浏览器


所选版本


简单介绍






Internet Explorer


8.0.6001.18702


microsoft公司出品。依靠捆绑在Windows里，而成为浏览器龙头老大，70+%的市场占有率。






Firefox


中国版 3.0.8


Moliza公司出品。依靠插件等特性成为唯一站在一线和IE对抗的浏览器，但始终差距明显。






Safari


4 Public Beta (528.16)


Apple公司出品。在MAC OS上一直使用着，windows版较之比较冷。但是它很酷。






Opera


9.64 build10487


Opera公司一直生产着浏览器，而且涉及各个平台，LINUX、手机等。JAVA平台。






Chrome


2.0.174.0


Google公司出品。号称因为市场上没有好的浏览器所以才开始研发的浏览器。






Maxthon2


2.5.2.5442


傲游是一个民间浏览器，依靠着捐赠在开发的团队。我在Chrome之前一直用它，从1开始。






Maxthon3


3.0.0.103 Alpha2


祸由他起，就是因为MX3的新版本让我很兴奋，才更新了这么多文章。Alpha版，开发中。






世界之窗


2.4.0.9 Final


凤凰工作室。不太了解，只是用的人比较多而已，就一起当小白鼠玩玩咯～






![](/images/uploads/zb/2009-04-19_browsers.png)




好的，这样小白鼠就站成好了等着我的尖牙利爪来检测了。测验总共是两个。






  1. CSS3的标准测试：主要是CSS的支持度，一共是43个大项578个小项的测试，红色是不通过，黄色是有点问题，绿色就是通过了说。这个测试只是一方面，它不是关键，主要还是ACID3的测试，比较全面。


  2. ACID3标准测试：W3C标准。包括DOM,CSS,JS,HTML等等等等的测试。下面是全部 = =




  * DOM2 Core


  * DOM2 Events


  * DOM2 HTML


  * DOM2 Range


  * DOM2 Style (getComputedStyle, …)


  * DOM2 Traversal (NodeIterator, TreeWalker)


  * DOM2 Views (defaultView)


  * ECMAScript


  * HTML4 (<object>, <iframe>, …)


  * HTTP (Content-Type, 404, …)


  * Media Queries


  * Selectors (:lang, :nth-child(), combinators, dynamic changes, …)


  * XHTML 1.0


  * CSS2 (@font-face)


  * CSS2.1 ('inline-block', 'pre-wrap', parsing…)


  * CSS3 Color (rgba(), hsla(), …)


  * CSS3 UI ('cursor')


  * data: URIs


  * SVG (SVG Animation, SVG Fonts, …)




![](/images/uploads/zb/2009-04-19_CSS3test.png)

	![](/images/uploads/zb/2009-04-19_ACID3test.png)




好的，8只小白鼠要过两关（无视第一关也行，因为情况相同），那么看看最后的结果吧。










浏览器


CSS3


ACID3






Internet Explorer


From the 43 selectors 22 have passed, 1 are buggy and 20 are unsupported (Passed 349 out of 578 tests)


20/100






Firefox





From the 43 selectors 36 have passed, 0 are buggy and 7 are unsupported (Passed 373 out of 578 tests)





71/100






Safari


From the 43 selectors 43 have passed, 0 are buggy and 0 are unsupported (Passed 578 out of 578 tests)


100/100






Opera


From the 43 selectors 43 have passed, 0 are buggy and 0 are unsupported (Passed 578 out of 578 tests)


85/100






Chrome


From the 43 selectors 43 have passed, 0 are buggy and 0 are unsupported (Passed 578 out of 578 tests)


100/100 (LINKTEST FAILED)






Maxthon2


From the 43 selectors 13 have passed, 4 are buggy and 26 are unsupported (Passed 330 out of 578 tests)


13/100






Maxthon3


From the 43 selectors 43 have passed, 0 are buggy and 0 are unsupported (Passed 578 out of 578 tests)


99/100 (右上角一个小红叉)






世界之窗


From the 43 selectors 13 have passed, 4 are buggy and 26 are unsupported (Passed 330 out of 578 tests)


13/100









世界之窗和Maxthon2最差，基于IE内核，并且对IE8的新特性没有及时加入，于是就成为了最低最低的东西了。








IE8紧随其后倒数第三，IE7开始支持CSS2，那么IE8究竟对CSS3有多好的兼容性呢，看来还是好不到哪里去。再加上其他的测试，唉，惨不忍睹了，还说回归标准。








火狐这一版本对CSS3的兼容是36/43，ACID3最后是71，是个合格的中等好少年了～








Opera得到了CSS3的满分，不过在其他方面的原因，最后是85，优秀了～








Maxthon3都能达到99分，就差了那么一点点，有时候也能出来个100分。不过这是MX3的极速模式，就是新内核下。切换到兼容模式（IE内核），那就变成了和MX2一样的13/43和13/100了。我怀疑新内核的良好表现来自chromium的帮助吧。








Chrome虽然能达到100，不过还是有点小问题，而且在第一次1.0几道现在一直没有修补掉，不知道是不是出于安全考虑，因为看上去像是链接方面的。








Safari4很意外的顺畅的直接飙到100分，好不爽快，看来苹果公司在标准支持上下足了功夫，值得大家学习啊！




好了天天更新着无聊的文章，大家看看热闹就好了。本来是想做个技术博客啊 T T 变成小白鼠实验室了。不如改名字算了。
