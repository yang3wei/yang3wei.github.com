---
layout: post
title: "(转载)很好的一篇 git 入门文章"
date: 2013-02-02 00:00
comments: true
categories: git相关
tags: [git]
---
原文链接：  
[http://blog.csdn.net/ylm23_24/article/details/8561527](http://blog.csdn.net/ylm23_24/article/details/8561527)  
用了几次 git 了，一直没总结，还是不太熟。  
下载和安装什么的就不说了。  
Windows 下推荐 git 和 tortoise git。Linux 下的 gitk 貌似还不错。 
<!-- more -->
###1.本地创建新仓库
创建新文件夹，打开终端执行  
`git init`  

这样就创建了一个新的 git 仓库  

###2.复制仓库
本地复制，创建一个本地仓库的克隆版本。  
`git clone /path/to/repository`  

将远端服务器上的仓库复制到本地，就跟下载差不多，如下例  
`git clone git@github.com:linus-young/depot.git`  

###3.添加与提交
`git add filename`  
`git commit -m "代码提交信息"`  

###4.推送到远端服务器
在 github 上申请一个帐号，create a new repo，  
创建之后其实 github 上有提示要执行哪些命令，注意两个邮箱需一致。  
至于本地的配置详见 [http://blog.csdn.net/ylm23_24/article/details/8297362](http://blog.csdn.net/ylm23_24/article/details/8297362)  
在当前本地文件夹下执行：  
`git remote add origin git@github.com:linus-young/depot.git`  
`git push origin master`  

第一行代码是确定添加到 github 的哪个仓库，作为第一次提交。  
第二行是将本地的所有文件上传到远端服务器上。  
master 是默认分支，可改为其他分支。本地创建的其他分支默认是不可见的。  

###5.分支
master 是默认的。在其他分支上进行开发，完成后再合并到主分支上，有利于多人共同开发。  
在当前本地仓库新建一个分支 test  
`git branch test`  
若此时运行  
`git branch`  

可以看到所有当前存在的分支  
test  
\*master  

切换到 test 分支  
`git checkout test`  
现在来随便更改一些文件，并且提交，然后切换到 master  
__(edit file)__  
`git commit -a -m "try branch"`  
`git checkout master`  

切换之后你打开刚刚修改过的文件，神奇的是貌似这些文件都没有被修改过！  
原因是你此时处于 master 分支，test 分支里面所作的改动是不起作用的，不信的话可以用  
`git checkout test`  
然后看看文件是否被修改了  

然后你可以修改 master 分支下的一些文件并且提交  
__(edit file)__  
`git commit -a -m "i just edit some file on master"`  

然后我们来合并 test 分支上的改动到 master 下  
`git merge test`  

报错的话可以用 git diff 查看冲突  
然后提交  
`git commit -a -m "i have merged test to master"`  

下面这行可以删除 test 分支：  
`git branch -d test`  

###6.其他常见命令
`git status` 查看文件状态  
`git log`   查看提交的历史记录  
`git pull` 和 `git push` 相反  

###7.gitk
`gitk` 可以用图形化的方式很清楚地显示改动  



