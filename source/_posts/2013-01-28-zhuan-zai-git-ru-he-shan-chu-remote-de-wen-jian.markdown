---
layout: post
title: "(转载)Git 如何删除 Remote 的文件"
date: 2013-01-28 17:25
comments: true
categories: Git相关
tags: [git]
---
原文链接：[http://hi.baidu.com/zairl23/item/4a34c60084108fd01ef0464d](http://hi.baidu.com/zairl23/item/4a34c60084108fd01ef0464d "The Way")  
使用 toto 做了个博客挂着 heroku 上。  
使用 git push 的方法发布博客确实有点cool。  
可是发现以前删除的测试文件仍然在, 如何将他们通过 git 的方式删除呢?  
不止是 heroku, 我发现 github 上也保留有我删除掉的文件.  
由于某些原因，git 远程的文件与本地文件产生了不一致，现在需要删除远程的一个文件：  
<!-- more -->
通过使用：  
<pre><code>git status</code></pre>  
得到下面的信息：  
**\# On branch master  
\# Changes not staged for commit:  
\#   (use "git add/rm <file>..." to update what will be committed)  
\#   (use "git checkout -- <file>..." to discard changes in working directory)  
\# deleted:    app/assets/stylesheets/base.css  
\# deleted:    app/assets/stylesheets/blue.css  
 no changes added to commit (use "git add" and/or "git commit -a")**  
  
即是说，base.css 和 blue.css 两个文件已经在本地删除，却因为没有 commit，而还在远程的 git 里面了，那么重新执行：  
<pre><code>git rm app/assets/stylesheets/base.css</code></pre>  
显示：  
**rm 'app/assets/stylesheets/base.css'**  
  
<pre><code>git rm app/assets/stylesheets/blue.css</code></pre>  
显示：  
**rm 'app/assets/stylesheets/blue.css'**  
  
现在要 commit 了：  
<pre><code>git commit -m 'delete base-blue css'</code></pre>  
显示：  
**[master 41124a8] delete base-blue css
2 files changed, 0 insertions(+), 690 deletions(-)
delete mode 100644 app/assets/stylesheets/base.css
delete mode 100644 app/assets/stylesheets/blue.css**  
  
然后提交：  
<pre><code>git push origin master</code></pre>>  
  
ok 了。总结一下：  
多运行 git staus 查看有没有未曾 commit 过的文件，  
因为在 git 里面，只有 commit，你才能够 push 成功的!  



