---
comments: true
date: 2012-03-02 00:07:32
layout: post
slug: git-prune-branch
title: git修剪分支
wordpress_id: 780
categories:
- Programing
tags:
- git
---

项目运转的久了，很多个人的战略的bugfix的feature的分支一堆一堆，今天忍不住想清理一些已经被合并的分支。这里用到了几个相关的git命令。





	
  * `git branch` 可以查看本地有的分支，当前分支之前有个*。

	
  * `git branch -r` 查看所有远程的分支（不代表远程还有的分支）

	
  * `git branch -a --color` 查看所有分支（`--color`加颜色，绿色是tracking的）

	
  * `git branch -d TAG` 删除本地分支，如果这个分支有没有合并的提交，git会提示你改用 `-D` 强制删除（或干脆别删除！）

	
  * `git push REMOTE :TAG` 删除远程REMOTE（比如一般叫origin）分支，注意分支名前加冒号

	
  * `git remote prune REMOTE` 这个指令比较特殊，如果你在A仓库上`git fetch`过，在B仓库用上一条命令删除了远程分支，那么A仓库里 `git branch -r` 还是可以看到已经删掉的分支。这个时候可以用这个命令修剪这些分支。

	
  * `git prune ` 在本地也可以使用，不过作用我还真不清楚，官方用推荐用 `git gc `




记录下，git真的很萌的你们不要黑他。




以上。



