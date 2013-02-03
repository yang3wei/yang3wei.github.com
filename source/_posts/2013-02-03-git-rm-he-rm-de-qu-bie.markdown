---
layout: post
title: "\"git rm\" 和 \"rm\" 的区别"
date: 2013-02-03 14:01
comments: true
categories: Git相关
tags: [git]
---
这是一个比较肤浅的问题，但对于 git 初学者来说，还是有必要提一下的。  
<!-- more -->
用 `git rm` 来删除文件，同时还会将这个删除操作记录下来；  
用 `rm` 来删除文件，仅仅是删除了物理文件，没有将其从 git 的记录中剔除。  

直观的来讲，`git rm` 删除过的文件，执行 `git commit -m "abc"` 提交时，  
会自动将删除该文件的操作提交上去。  

而对于用 `rm` 命令直接删除的文件，执行 `git commit -m "abc"` 提交时，  
则不会将删除该文件的操作提交上去。  
不过不要紧，即使你已经通过 `rm` 将某个文件删除掉了，  
也可以再通过 `git rm` 命令重新将该文件从 git 的记录中删除掉，  
这样的话，在执行 `git commit -m "abc"` 以后，也能将这个删除操作提交上去。  

如果之前不小心用 `rm` 命令删除了一大批文件呢？  
是的，此时用 `git rm` 逐个地再删除一次就显得相当蛋疼了。  
所幸还有更方便的处理方案，用如下的方式做提交就没有问题了：
`git commit -am "abc"`  

###总结一下：
在被 git 管理的目录中删除文件时，可以选择如下两种方式来记录删除动作：  
一、`rm` + `git commit -am "abc"`  
二、`git rm` + `git commit -m "abc"`  
另外，`git add .` 仅能记录添加、改动的动作，删除的动作需靠 `git rm` 来完成。  
最后，`rm` 删除的文件是处于 `not staged` 状态的，  
也就是一种介于 “未改动” 和 “已提交过” 之间的状态。  

###下面是测试图
一、`git add .` 无法记录 `rm` 删除动作  
{% img center /images/posts/add_dot_make_no_use.png %}
二、`git commit -m "abc"` 无法提交 `rm` 删除动作  
{% img center /images/posts/git_rm_better_than_rm.png %}
三、`git commit -am "abc"` 中参数 a 的作用  
{% img center /images/posts/usage_of_flag_a.png %}
