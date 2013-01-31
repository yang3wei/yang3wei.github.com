---
layout: post
title: "(扩充)为 octopress 添加标签云"
date: 2013-01-30 02:54
comments: true
categories: octopress相关
tags: octopress
---
原文链接：[http://codemacro.com/2012/07/18/add-tag-to-octopress/](http://codemacro.com/2012/07/18/add-tag-to-octopress/ "loop in codes")  
同添加 `category list` 一样，网络上有很多方法，这里列举一种。  
首先将以下两个项目克隆到本地：
<pre><code>https://github.com/robbyedwards/octopress-tag-pages 
https://github.com/robbyedwards/octopress-tag-cloudclone
</code></pre>
这两个项目分别用于产生 `tag page` 和 `tag cloud`。  
针对这两个插件，需要手工复制一些文件到你的 octopress 目录。  
<!-- more -->
###1.octopress-tag-pages
复制 `tag_generator.rb` 到 `/plugins` 目录。  
复制 `tag_index.html` 到 `/source/_layouts` 目录。  
复制 `tag_feed.xml` 到 `/source/_includes/custom/` 目录。  
其他文件就不需要复制了，都是些例子。

###2.octopress-tag-cloud
复制 `tag_cloud.rb` 到 `/plugins` 目录。  
复制以下代码到 `/source/_includes/custom/asides/tags.html`。  
__注意：先去掉 % 前面的反斜杠__
<pre><code>&lt;section&gt;
    &lt;h1&gt;Tags&lt;/h1&gt;
    &lt;ul class="tag-cloud"&gt;
        {\% tag_cloud font-size: 100-210%, limit: 49, style: para \%}
    &lt;/ul&gt;
&lt;/section&gt;
</code></pre>
`tag_cloud` 的参数中：  
`style: para` 指定不使用 `li` 来分割；  
`limit` 限定 10 个 tag；  
`font-size` 指定 tag 的大小范围，具体参数参看官方文档。  

最后，当然是在 `_config.yml` 的 `default_asides` 中添加这个 `tag cloud` 到导航栏，例如：  
<pre><code>default_asides: [asides/category_list.html, asides/recent_posts.html, custom/asides/tags.html]</code></pre>

###3.本人扩充: 有一个要注意的地方
结合自身经历，我按照前文所述操作且核对了很多次，  
发现在 `rake generate` 和 `rake preview` 以后，  
在预览页面却依然无法得到正常的结果，具体表现如下：  
标签云确实是出现在了右栏里面，不过当我点击某个 tag 的时候，  
跳转到的却不是有效的地址，而仅仅只是一个 tag 关键词！  
灰常地荒谬，灰常地震惊，我甚至一度思忖，作者是不是故意在和我开玩笑。  
经历了艰辛地排查，终于我在 `firebug` 下面发现了一些蛛丝马迹：  
常规情况下 tag 对应的地址应该是 `/tags/关键字`，  
但是我发现在我的站点上面却不是这样的，仅仅只有 `//关键字`，  
体现在浏览器里面就仅仅是一个 tag 关键字了，因为两个斜杠都被滤掉了~  
发现了问题的症结以后，我就跑到 \_config.yml 配置文件去中查看，  
并且拿自己的配置文件和其他正常人的配置文件作对比，果然发现了一些猫腻：  
我的配置文件中少了一行配置：`tag_dir: tags`，也算是歪打正着了，  
把这一行配置加进去之后，执行 `rake generate` 和 `rake preview` 命令，  
转到浏览器里面键入 `localhost:4000` 进行查看，标签云的功能总算是回归正常了~  

谈一谈我对此的感受吧：  
之前我也没有删掉过 tag\_dir 这一行配置，也就是说 octopress 本身就没有包含这一行配置~  
我很奇怪为什么其他人竟然都没有提出甚至谈及这个问题，  
难道是 octopress 在升级的过程中刚才将这一行配置去掉？  
不论如何，此次经历还是让我相当的不愉快，我花了至少五个小时来解决这个问题！！  
这让我难以接受，要知道此时此刻我本该已经躺在床上呼呼大睡了！  
直言不讳吧，虽然拿 MacVim 写 markdown 很舒服，但我不得不承认：  
octopress 毕竟还是没能达到我心目中理想的高度，此次就是不成熟的一个典型体现~  



