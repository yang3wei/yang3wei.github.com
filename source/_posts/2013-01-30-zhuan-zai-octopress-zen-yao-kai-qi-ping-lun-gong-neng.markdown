---
layout: post
title: "(扩充)Octopress 怎么开启评论功能"
date: 2013-01-30 18:09
comments: true
categories: octopress相关
tags: octopress
---
原文链接：  
[http://gangmax.me/blog/2012/01/20/how-to-use-disqus-in-octopress/](http://gangmax.me/blog/2012/01/20/how-to-use-disqus-in-octopress/ "Blog of GangMax")  
###1.Octopress 默认支持 disqus，开启即可
<!-- more -->
这两天我一直想给 octopress 加入评论功能。  
于是我尝试搜索 `disqus octopress jekyll` 这样的关键字，但是没有找到具体的做法。
后来我想到：可以查看一个具有评论功能的 github octopress 实例代码，看看别人是怎么做的，比如[这个](https://github.com/roylez/roylez.github.com)。

我首先查看 `source/_layout/post.html`，看到里面有处理 disqus 的相关代码。  
我的第一反应是我应该在我自己的对应文件里面也加上相应的代码。  
但是随即发现我的文件中已经有了一模一样的代码。  

也就是说，其实 `octopress/jekyll` 默认就有这些代码。  
这说明 octopress 自身就支持 disqus，  
可能这就是为什么没有人评论该怎么在 octopress 里面加入 disqus 支持的原因。

于是打开 `_config.yml`，找到了 disqus 相关的配置项，修改即可：  
<pre><code>disqus_short_name: your_disqus_short_name
disqus_show_comment_count: true</code></pre>
当然，前提是你需要先注册一个 [disqus](http://www.disqus.com/) 帐号，这个就不用我多说了。

###2.一个要注意的地方
原文请参看如下链接：  
[http://www.ducea.com/2012/11/12/disqus-comments-not-visible-in-octopress/](http://www.ducea.com/2012/11/12/disqus-comments-not-visible-in-octopress/ "MDLog:/sysadmin")
大意就是在更改 octopress 配置文件 \_config.yml 时，  
下面两者有很大的区别，后者多加了一个斜杠将直接导致看不见 disqus 的评论内容！
__`url: http://yang3wei.github.com`    
`url: http://yang3wei.github.com/`__
  



