---
layout: post
title: "git-credential-osxkeychain died of signal 11 解决方案"
date: 2013-02-03 00:30
comments: true
categories: Git相关
tags: [git]
---
相关帖子链接：  
[...osxkeychain-died-of-signal-11@stackoverflow.com](http://stackoverflow.com/questions/14272634/error-git-credential-osxkeychain-died-of-signal-11/14663780#14663780)  
前两天在执行 `git push origin master` 命令的时候突然发现不灵光了，  
死活都推不进 GitHub 服务器，给出的提示如下：  
`error: git-credential-osxkeychain died of signal 11`  
`error: git-credential-osxkeychain died of signal 11`  
<!-- more -->
我没有弄错，给出了两行一模一样的提示。  
遇到这个问题以后我的第一反应就是 googling，  
直接就找到了文首给出的那个 stackoverflow 的帖子。

不过这个帖子并没有帮我解决掉这个问题，然后我又继续 googling，  
没有太大的收获，涉及到相关问题的链接不是很多。。  
我一度多次打开 stackoverflow 的这个帖子，仔细的搜寻，  
想看一看我是否遗忘了什么重要的步骤，不过很可惜，没有发现有价值的情报。  

后来我计上心头，想到了一个好办法，  
去试一试另外一个本地目录的 work copy 能否推到 GitHub 服务器里面去。  
我做了一些变更并且提交过以后，就执行 “git push origin master” 命令，  
很奇葩的没有出错，也没有向我询问用户名和密码~  

到这里我就有点儿纳闷儿了，凭什么这里可以那儿却不行？！  
纠结了好几天了，今天突然又想到一个办法，  
怎么不对比一下两个 work copy 的 .git/config 文件呢，说不定有有所收获！  
果不其然，经我对比过后我发现两个 work copy 所采用的配置有所不同。  
区别在哪里呢？下面我给出来对比一下吧：  
__这里是 push 时抛错的那个 work copy 的 .git/config 文件的部分配置__
<pre><code>...
[remote "origin"]
    url = https://github.com/yang3wei/octopress-3-in-one.git
    fetch = +refs/heads/*:refs/remotes/origin/*
...
</code></pre>
__这里是 push 时没问题的 work copy 的 .git/config 文件对应的部分配置__
<pre><code>...
[remote "origin"]
    url = git@github.com:yang3wei/game-development-tools
    fetch = +refs/heads/*:refs/remotes/master/*
...
</code></pre>

对比后我就多少有些念想了，何不试着把有问题的照着没问题的改弄一翻？  
说干就干，于是我就将有问题的那块儿配置改成了如下这番：  
<pre><code>
[remote "origin"]
    url = git@github.com/yang3wei/octopress-3-in-one
    fetch = +refs/heads/*:refs/remotes/origin/*
...
</code></pre>
然后再执行 `git push origin master` 命令，发现那该死的错误已经悄然遁去~  

###总结一下
一言以蔽之，就是更换了一下 .git/config 中 url 的写法而已，  
将 `url = https://github.com/yang3wei/octopress-3-in-one.git`  
改为 `url = git@github.com:yang3wei/octopress-3-in-one`  
说到这里，我隐约想起好像看到过一句话：  
ssh 加密传输好像是非后面那种 url 格式不可的，  
那么从这点来看的话，  
`git-credential-osxkeychain` 在 https 传输模式下出错也就情有可原了~  
最后，创建属于自己的 repo 的时候，  
最好还是能以 `git@github.com...` 这种 url 的写法来初始化仓库，  
以避免当前讨论这种问题的出现~  

###另外
其间也试了下将  
`fetch = +refs/heads/*:refs/remotes/origin/*`  
改为  
`fetch = +refs/heads/*:refs/remotes/master/*`  
如此这般以后我执行 `git push origin master` 也没有出现错误~  
不过后来在我查看的时候发现竟然多出了一个分支：  
`* master`  
`  remotes/master/master/`  
`  remotes/origin/master/`  
显而易见，那里我不该将 fetch 里面的 origin 改成 master，  
这里直接导致我远程那边多出了一个无用的分支来~  
话说这里该如何将这个多余的远程分支 `remotes/master/master` 给干掉呢？  
记得删除远程服务器上面的分支可以用这条命令：`git push origin :branchname`  
不过这里比较特殊，这条多余分支的名字也是 `master` 呢！  
搜寻良久，stackoverflow 再次帮了我一把，实在是给力啊！下面是相关链接：  
[How do you Remove an Invalid Remote Branch Reference from Git?](http://stackoverflow.com/questions/1072171/how-do-you-remove-an-invalid-remote-branch-reference-from-git)  
罗列出我所尝试的删除分支的命令：  
`git branch -d remotes/master/master` 报错  
`git push master :master` 报错  
`git push origin :/remotes/master/master` 报错
`git gc --prune=now` 无效  
`git remote rm master` 报错  
`git branch -rd master/master` 删除成功！  

###执行 git status 命令后出现如下提示的涵义  
<pre><code># On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#   (use "git push" to publish your local commits)
#
nothing to commit, working directory clean
</code></pre>
回忆起前几天对 git 不熟悉时因为老是看到这几行提示而感到心里不舒服的情景，  
才发现我真是傻得可爱，这些提示的意思不就是说：  
__你当前有 1 个已完成的提交还没推到远程服务器，执行 push 命令来推出已完成的提交__  
哎，那天也不知道是怎么了，肯定是发现和平时不一样却没有仔细看就匆忙下了定论，  
今后得把这种粗细大意的毛病给改掉~  



