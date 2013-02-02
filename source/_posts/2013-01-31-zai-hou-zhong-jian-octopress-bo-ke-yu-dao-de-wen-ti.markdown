---
layout: post
title: "灾后重建 octopress 博客遇到的问题"
date: 2013-01-31 16:58
comments: true
categories: Octopress相关
tags: [octopress]
---
今天上午十点的时候试了一下 `git pull` 命令，  
结果导致 `octopress` 博客在我本地的目录乱成了一团浆糊，  
因为对 `git` 的了解不是很充分，所以在多番努力修复无果之后，我决定重新洗牌。  
<!-- more -->
话说 `git` 的分支、合并什么的确实有些不容易理解~  
其实 `blog` 还是能够正常使用的，但是我本地 `octopress` 的目录被完全破坏掉了，  
失去了我对它的掌控，另外就是，在提交的时候老提示类似如下的信息  
<pre><code># On branch master
# Your branch is ahead of 'origin/master' by 1 commit.
#
nothing to commit (working directory clean)</code></pre>
我是一个追求完美的人，这些个多余的东西让我浑身不自在，于是我有了 `重装` 的想法，  
重装其实也有其他的用意，比如说，让我对架设 octopress 博客的流程更加轻车熟路些。  
因为之前走过一遍，所以我在做费时评估时是准备在 10 分钟内搞定的。  

但是现实往往是那么地不可预料，这不，在重装的过程中又遇到了一些问题，- -、  

###第一步，清除所有陈旧的东西
将 `octopress` 博客的本地目录拽入垃圾箱；  
在 `github` 里面删除 `yang3wei.github.com` 博客仓库。  
ok，就这么多！  

###第二步，在 github 上面重建 yang3wei.github.com 仓库
这个不多说，`github` 官网上面有做全方位地向导。  

###第三步，重新布置 octopress 博客的本地目录
这个也没什么好说的，顺着 `octopress` 主页底部的 `start here` 链接一路往下走即可。  

但是这里面有猫腻，如果处理的不好，将导致管理 octopress 博客出现一些混乱！  
这里说一下存在问题的处理方式：  
按照 `octopress` 主页上面介绍的搭建流程，  
我开启一个 `Termimal` 窗口，直接在里面粘贴并执行如下的命令：  
`git clone git://github.com/imathis/octopress.git octopress`  
`Terminal` 窗口开启的时候默认的所在目录为 `~`，  
上面的命令会将 `octopress` 的安装文件克隆到本地的 `~/octopress` 目录。  
之后我规规矩矩地执行下列命令：`cd octopress`、`rake install`，  

然后我点击 `Next Steps` 下面的 `Set up deployment` 链接进入到创建 `github` 博客仓库的环节。  
按照 `octopress` 给出的提示，我在 `github` 里面创建了一个名为 `yang3wei.github.com` 的仓库。  
之后我复制好 `github` 所生成的命令文本：
<pre><code>touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/yang3wei/yang3wei.github.com.git
git push -u origin master
</code></pre>
二话不说跑到之前打开的那个 `Terminal` 窗口里面就粘贴执行了，  
但在执行这组命令时并没有想象中的顺利，在执行倒数第二行命令的时候出现了故障：  
`fatal: remote origin already exists`  
`googling` 一番以后找到问题的解决方案，在执行倒数第二行命令前先执行一遍如下命令：  
`git remote rm origin`  
然后，依次粘贴执行前述命令块儿的最后两条命令，其间没有再生出其他的枝节。  
问题解决方案的相关链接：[Github “fatal: remote origin already exists”](http://stackoverflow.com/questions/10904339/github-fatal-remote-origin-already-exists)

做完了上面的操作，就算是将 `github` 上 `yang3wei.github.com` 仓库的本地目录给布置好了。  
之后点进 [Deploying to Github Pages](http://octopress.org/docs/deploying/github/) 链接，继续按照提示往下走，  
我在之前提到的那个 `Terminal` 窗口里面再次粘贴并执行如下命令块儿：  
<pre><code>rake setup_github_pages
rake generate
rake deploy
git add .
git commit -m "first commit"
git push origin source
</code></pre>
在依次执行到最后一条命令 `git push origin source` 时，  
问题再次降临，本地提交的数据死活推不进 `github` 服务器。  
我用 `git status` 和 `git branch -a` 命令查看了一下当前的分支状态和所有的分支条目，  
发现 `source` 分支根本就不存在，当前所处的也只是 `master` 分支。  
我感到非常的不可思议，因为在我之前顺利搭建 `otopress` 博客的时候，  
我是一直都工作在 `source` 分支下面的，现在却仅仅只有一个 `master` 分支！  
我重复删除创建实践了很多次，最终却都只得到上述的结局。  
有几次我忍不住按照 `git` 给出的提示执行了 `git pull` 命令，结果一下就完蛋了：  
静态页数据直接被拉到本地的 `octopress` 根目录，把根目录弄得一团乱麻。   

###真相在哪里？
熟悉 git 运作机制的看官可能已经发现问题的所在了！  
我不明所以地将 `yang3wei.github.com` 仓库的本地目录和 `~/octopress` 目录重叠了起来。  
正是因为这一点导致了我 `n + 1` 次的重建失败！正确的处理方法：  
__使用除 `~/octopress` 目录之外的其他目录作为 `yang3wei.github.com` 的本地目录。__  
庆幸我是一个执着的人，没有 `n + 1` 次的失败，哪有第 `n + 2` 次的成功？  





