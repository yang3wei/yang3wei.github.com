---
layout: post
title: "Git diff 代码比较的高级技巧"
date: 2013-02-15 00:24
comments: true
categories: Git相关
tags: [git]
---
原文链接：[Git diff代码比较的高级技巧](http://blog.csdn.net/offbye/article/details/6592563)  
新的 android 的项目涉及到 android 的源码的管理和修改，我们是在 android 源码基础上做 TDSCDMA 和 GSM 的双卡双待功能实现，项目中使用了Git 作为版本管理工具，因此借此机会深入研究了 Git 的原理和使用方法。这里重点说一下 Git diff 相关的技巧。  
<!-- more -->
Git 是使用 branch 来管理不同的功能点开发的，那么我们怎样能比较不同 branch 的不同呢？  
使用 git diff branch1 branch2 , 就可以了，但这个方法不够直观，因为只能显示不同点的上下几行，不方便理解。  
比较好的做法是使用图形化比较工具比较，例如 meld，使用以下的命令就可以了  
```
git difftool -t meld -y   branch1 branch2
```
这样可以使用 meld 一个一个文件的比较，每次关闭 meld 就会自动显示下一个不同的文件。  

比较不同的 commit，使用以下命令就可以了  
```
git difftool -t meld -y  commitId1  commitId2
```
比较工作区和上次提交的差异，这个最常用了  
```
git difftool -t meld -y  HEAD
```
你可以使用 git config 命令设置 meld 为默认的比较工具，并且把 prompt 设为 false，这样以后就可以使用 git difftool 了。  
也可以直接修改 .gitconfig  
gedit ~/.gitconfig 在最后加入  
```
[diff]
    tool = meld
[difftool]
    prompt = false
```
当然了，如果你不喜欢 meld，也可以使用其他的比较工具，git difftool 支持以下的比较工具：  
kdiff3, kompare, tkdiff, meld, xxdiff, emerge, vimdiff,   
gvimdiff, ecmerge, diffuse, opendiff, p4merge and araxis  
