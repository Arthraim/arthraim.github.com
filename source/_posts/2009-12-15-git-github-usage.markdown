---
comments: true
date: 2009-12-15 14:39:13
layout: post
slug: git-github-usage
title: Git/Github使用方法小记
wordpress_id: 126
categories:
- Programing
tags:
- AIR
- 人间网
- git
- github
- open source
- renjian-deck
---

今天把人间网的桌面客户端renjian-deck正式开源了，之前对javascript的了解其实非常的不够的，所以这一次的代码写的也是乱七八糟重用性及其低下，虽然我无数次的想把代码重新整理一下，不过还是糊里糊涂一时冲动的在他们还是乱七八糟的时候开源了。因为之前是基于github上的一个开源项目，所以硬着头皮也放到github上，虽然没有使用的经验，不过磨机磨机还是搞定了。




以下是具体步骤，就当是自己做个笔记了，高手请绕道吧。




**1、下载安装桌面端git。**




Windows请至：[http://code.google.com/p/msysgit/](http://code.google.com/p/msysgit/)




安装的时候最好还是允许在shell嵌入git的命令，相对还是比较方便的。




**2、git的初始设置**




    git config --global user.name "Your Real Name"
    git config --global user.email you@email.address




**3、建立仓库**




在git bash里找到你的项目目录。（或直接用shell右键里的git bash here）




    git init




这样在你的项目目录下就会有一个.git的隐藏目录（类似于.svn） 。




**4、初始化项目**




    git add .




留心后面的一个 "." ， 这是添加所有文件的情况，如果愿意，你也可以添加特定的几个文件，比如git add readme.txt等等。




之后就可以做我们的first commit到仓库里了。




    git commit -m 'first commit'




-m 参数以及后面的字串是添加说明。




**5、 注册github账号**




下面就是与github有关的操作了。




首先到http://github.com/注册账号。注册之后可以看到这样的界面。选择第一项创建一个项目。




![](/images/uploads/zb/2009-12-15_github_dashboard.jpg)




表单需要填写






  * Project Name（项目名称）


  * Description（描述）


  * Homepage URL（主页URL，一般就以项目名称命名好了）




**6、创建SSH密匙**




这步工作应该是最麻烦的吧。回到桌面，打开git bash，输入以下命令。




    ssh-keygen -C 'your@email.address' -t rsa




确认使用默认路径，然后输入两次你要是用的密码就行。




可以使用以下命令测试连接




    SSH -v git@github.com




会要求输入你刚才设置的密码，如果成功的话可以看到这样的ERROR（orz，起码证明连接是成功了）




    ERROR: Hi Arthraim! You've successfully authenticated, but GitHub does not provide shell access




**7、提交密匙**




现在又要回到github的页面上，在右上方工具栏里找到Account Settings。在这个页面上有一个SSH Public Keys标签，选择Add another public key。Title随便取，Key是一段东西。




找到刚才创建密匙的那个目录下（默认是C:Documents and Settings你的windows用户名.ssh）找到id_rsa.pub文件，把它打开可以看到一堆文字，拷贝下来黏贴到github页面key的空白处。然后Apply，就好了。




**8、上传代码**




最后就是上传你的代码了~ bash切换到你的项目目录下，输入以下命令。




    git remote add origin git@github.com:你的github用户名/你的github项目名.git
    git push origin master




hehe，现在再去"http://github.com/你的github用户名/你的github项目homepage Url" 就可以看到你的项目了~ Good luck




当然这是从无到有，如果你有一个git的repo，想添加到github上，那就直接使用伟大的第8步的命令就可以了（不要忘记密匙的相关工作）。话说很多初学者应该会和我一样，在初期搞不清git和github的关系，git是和CVS,SVN并列的一个概念，而github是和Google Code, sourceforge并列的一个概念，这样说就明白了吧。所以，git的学习的话，参见[这里](http://www.linuxgem.org/2008/8/1/git-tutorial.4889.html)。




BTW：Github的社区感很好，体验很不错，怪不得有这么多人在github上乐此不疲的交流代码。




最后附图一张。




[![](/images/uploads/zb/2009-12-15_github_hint.jpg)](/images/uploads/zb/2009-12-15_github_hint.jpg)




**相关链接：**






  * [Getting Started with Git and GitHub on Windows](http://kylecordes.com/2008/04/30/git-windows-go/)


  * [windows下使用git管理github项目](http://hi.baidu.com/mcspring/blog/item/171b1e38986d39fab211c71b.html)


  * [Git使用指南](http://www.linuxgem.org/2008/8/1/git-tutorial.4889.html)


  * [renjian-deck on Github](http://github.com/Arthraim/renjian-deck/)







以上……
