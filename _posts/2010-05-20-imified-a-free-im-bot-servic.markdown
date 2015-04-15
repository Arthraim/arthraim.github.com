---
comments: true
date: 2010-05-20 11:04:05
layout: post
slug: imified-a-free-im-bot-servic
title: 免费的IM机器人服务—imified
wordpress_id: 439
categories:
- internet
tags:
- bot
- IM
- PHP
---

Google中游荡的时候发现了这个网站，[imified](http://imified.com)。这个网站可以免费的为你制作自己的IM机器人，支持gtalk, Jabber, MSN, AIM，甚至还有twitter和短信。




[![](/images/uploads/wp/logo_shine.png)](/images/uploads/wp/logo_shine.png)




要做的就是登陆网站，注册账号之后，创建一个自己的bot，取名，并且一定要有一个URL，放驱动你的bot的php程序文件，这就需要你有一个自己的空间。比如我注册了Arthurobot这个名字，然后在http://arthraim.cn/bot/arthurobot/ 下面放上index.php。利用网站提供的一些API，可以做一些自己的应用。比如照着例子写我的index..php。



```php
<?php
$msg=$_REQUEST['msg'];
$step=$_REQUEST['step'];
if($step > 3){
	switch($msg){
		case "hi":
		case "hello":
		echo "Hello there, I'm Arthurobot from Arthraim.cn";
		break;
	}
} else {
	switch ($step) {
	case 1:
	echo "Hello there, I'm Arthurobot from Arthraim.cn, so what's your name?";
	break;
	case 2:
	echo "Hi " . $_REQUEST['value1'] . ", where do you live?";
	break;
	case 3:
	echo "Well, welcome to this bot, " . $_REQUEST['value1'] . "<br>from " . $_REQUEST['value2'] . ".";
	break;
	}
}
?>
```



然后在gtalk加好友arthurobot@bot.im，和他对话，最终的效果就像这样。




[![](/images/uploads/wp/2010-05-20_bot_im_arthurobot_runtime.png)](/images/uploads/wp/2010-05-20_bot_im_arthurobot_runtime.png)




再之后他就只会回应hi和hello了，Arthur同学表示不会php，所以对我来说好像没有什么可以创造的东西。会php的同学可以做很多事情啊~




以上。
