---
layout: post
title: "Mac OS X 下 git 如何升级"
date: 2013-02-01 22:58
comments: true
categories: git相关
tags: [git, mac]
---
原帖链接：[http://segmentfault.com/q/1010000000095119](http://segmentfault.com/q/1010000000095119)  
`Mac OS X Lion` 下使用 `which git` 查看 `git`，  
发现当前所使用的 `git` 位于 `/usr/bin/git` 目录下，  
可能是安装 `XCode` 时一起安装上来的，执行 `git --version` 得到当前版本为：`1.7.5.4`。  

想升级到最新版本，下载 `git-osx-installer` 安装完成后，在命令行里查看却仍是旧版本。  
查阅资料发现这个安装包是将 `git` 安装在 `/usr/local/git` 目录下的。  

我想使用这个新版本的 `git`，该如何进行设置？

###解决方案：
其实是两个问题，第一个问题是高版本的 `git` 如何安装？  
用 `git-osx-installer` 也好，用 `brew` 也好，都可以。  
建议用 `brew` 安装：`brew install git`  

另一个问题是：如何让新安装的 `git` 覆盖老版本的 `git`？  
建议用 `~/.bash_profile`，加入以下的内容：  
`export PATH=/usr/local/git/bin:${PATH}`  
这样就可以让 `bash` 优先搜索 `/usr/local/git/bin` 下的指令，而且不会覆盖老文件，比较安全。  



