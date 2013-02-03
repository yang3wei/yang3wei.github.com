---
layout: post
title: "为 Octopress 添加查看访问量的功能"
date: 2013-02-03 16:03
comments: true
categories: Octopress相关
tags: [octopress]
---
原文链接：[sourrabbit & linpx](http://colors4.us/blog/2012/01/08/octopressbo-ke-cong-ling-kai-shi-iii/)  
我在看了[这个](http://www.whatwherewhy.me/colophon/)网页后，发现我还可以添加网站分析，毕竟是自己努力出来的东西，  
看看大伙是否有兴趣访问还是不错的，不过很多情况是远逊于预期。:)
<!--  more -->
先于 `Google Analytics` 开通和自己网站相关的服务，  
比如登记自己的网址，取得 GA 的 `Track ID`，应该如该样式 `UA-28584XXX-X`.
修改 `_config.yml` 最后部分，将 ID 置于 `google_analytics_tracking_id: `项目之后，rake一下就行了。  
之后进入 GA 网站看 report 吧，你就会知道何时多少人访问过你的网站。
