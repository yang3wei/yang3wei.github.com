---
layout: post
title: "git 笔记概览"
date: 2013-01-29 16:12
comments: true
categories: git相关
tags: git
---
<!-- more -->
###Git 如何删除 Remote 的文件
[Git 如何删除 Remote 的文件](http://yang3wei.github.com/blog/2013/01/28/zhuan-zai-git-ru-he-shan-chu-remote-de-wen-jian/ "(转载)Git 如何删除 Remote 的文件")

###git - 简易指南
[助你开始使用 git 的简易指南，木有高深内容，;)](http://rogerdudler.github.com/git-guide/index.zh.html "git - 简易指南")

###git 删除远程分支的命令
如果通过命令 `git branch -b feature_x source` 建立了 source 的一个分支 feature_x，  
而且还将 feature_x 这个分支提交到 github 服务器上面去了，  
此时由 feature_x 切换回 source 分支后执行 `git branch -d feature_x` 命令只能删除本地分支，  
怎么将 github 服务器上面的 feature_x 分支给干掉呢？  
执行 `git push origin :feature_x` 命令即可！  

###Git 分支管理策略
[Git 分支管理策略](http://yang3wei.github.com/blog/2013/01/29/zhuan-zai-git-fen-zhi-guan-li-ce-lue/ "Git 分支管理策略")  

###Git 怎么为 github 生成 ssh 密钥
[github:help - Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys "Generating SSH Keys")  
