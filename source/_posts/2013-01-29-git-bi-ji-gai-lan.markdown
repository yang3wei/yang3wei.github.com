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
执行 `git push origin :feature\_x` 命令即可！  

###Git 分支管理策略
[Git 分支管理策略](http://yang3wei.github.com/blog/2013/01/29/zhuan-zai-git-fen-zhi-guan-li-ce-lue/ "Git 分支管理策略")  

###Git 怎么为 github 生成 ssh 密钥
[github:help - Generating SSH Keys](https://help.github.com/articles/generating-ssh-keys "Generating SSH Keys")  

###Git 恢复删掉的一个文件
删除一个文件：  
`git rm 5.c`  
现在要恢复：  
`git reset HEAD 5.c`  
`git checkout 5.c`  
完成（HEAD 似乎表示当前分支的当前版本）

###Git 在各版本之间自由穿梭
恢复到上一个提交的版本  
`git reset --hard HEAD^`  
恢复到某一个提交的版本  
`git reset --hard e0dea1a7eaca4b9325e36fdbdf0909d02a067d43`  
__注：各版本的 `hash` 可以去 `github` 查看，也可以使用 `git log` 命令查看。__

###Git 怎么忽略某个文件
仓库的 `.gitignore` 或 `git/info/exclude`，`exclude` 本身不被 `git` 管理。  
一般情况下不要使用第一种方法，因为 `.gitignore` 本身是被 `git` 管理的，是大家共用的。  
所以，不要随便修改 `.gitignore` 文件！

###Yasin Lee 的 git 学习笔记
[http://blog.csdn.net/coder_jack/article/details/5975070#](http://blog.csdn.net/coder_jack/article/details/5975070#)  

###Problem with "git remote add origin git@github.com:yang3wei/test.git"？
试着先执行一下 `git remote rm origin` 命令。  
[github-fatal-remote-origin-already-exists](http://stackoverflow.com/questions/10904339/github-fatal-remote-origin-already-exists)  

###fatal: ... did you run git update-server-info on the server?
You have to carefully look after your spelling.   
According to Github's guide, your username is nalgene,   
hence the URL is `https://github.com/nalgene/MultiView.git`.   
The error message hints that you added the remote as   
`https://github.com/naglene/MultiView.git`   
which is not the same username, as you swapped the `l` and `g`.  
Also, the default branch is called `master`, not `maaster` or `mater`.  
[original link](http://stackoverflow.com/questions/11094547/fatal-https-github-com-user-repo-git-info-refs-not-found-did-you-run-git-upd)

###What does "origin" mean in "git push origin master"?
`git push origin master` 的完整命令如下：  
`git push git@github.com:{username}/{projectname}.git HEAD:{branchname}`  

Also, you don't need to type out the whole url each time you want to push.   
When you ran the clone, git saved that URL as `origin`,   
that's why you can run something like 'merge origin/test' -   
it means the `test` branch on your `origin` server.   
So, the simplest way to push to your server in that case would be:  
`git push origin my_test:test`  
That will push your local `my_test` branch to the `test` branch on your `origin` server.   
If you had named your local branch the same as the branch on the server,   
then the colon is not neccesary, you can simply do:  
`git push origin test`  
[Error when “git push” to github](http://stackoverflow.com/questions/959477/error-when-git-push-to-github)  

###
