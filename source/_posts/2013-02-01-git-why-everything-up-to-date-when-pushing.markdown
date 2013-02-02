---
layout: post
title: "(转载)Git: Why 'Everything up-to-date' when pushing"
date: 2013-02-01 23:22
comments: true
categories: Git相关
tags: [git]
---
原文链接：  
[http://blog.rexzhao.com/2011/11/28/google-code-git-everything-up-to-date-when-push.html](http://blog.rexzhao.com/2011/11/28/google-code-git-everything-up-to-date-when-push.html)  
第一次在 `Google Code` 上弄项目，注册完毕后，  
尝试增加一个新文件用以测试 Git 是否好好工作。  
结果在 `Push` 时却显示 `Every up-to-date`，检查文件时却发现实际上一个都没更新上去。  
<!-- more -->
因为对 `Git` 不够熟悉，因此只好 `Googling`，进行一番搜索后找到原因如下：  

__Why does Git refuse to push, saying "everything up to date"?  
git push with no additional arguments only pushes branches that exist in the remote already.   
If the remote repository is empty, nothing will be pushed.   
In this case, explicitly specify a branch to push, e.g. `git push master`.__  

也就是说一开始 `git` 服务器仓库是完全空的，  
不包含任何一个分支(`branch`)，因此刚开始 `Push` 时需要指定一个。  
执行 `git remote -v` 后看到自己的 `remote` 端名字为 `origin`:  
<pre><code>$ git remote -v
origin  https://code.google.com/p/micolog2 (fetch) 
origin  https://code.google.com/p/micolog2 (push)
</code></pre>
执行 `git branch` 后看到自己当下用的分支是 `master`:  
<pre><code>$ git branch 
* master
</code></pre>
因此在本地 `commit` 后，再执行 `git push origin master` 即可。  



