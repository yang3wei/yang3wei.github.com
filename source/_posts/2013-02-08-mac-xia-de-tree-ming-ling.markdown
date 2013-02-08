---
layout: post
title: "Mac 下的 tree 命令"
date: 2013-02-08 17:19
comments: true
categories: Mac相关
tags: [mac]
---
在默认安装安装的 `mac` 下没有找到 `tree` 命令，找了一下原来有个比较流氓的解决办法：  
<!-- more -->
{% codeblock %}
find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'
{% endcodeblock %}
这个命令行跑起来类似平常`tree`的效果，比如：   
{% codeblock %}
.
|____extra
| |____httpd-autoindex.conf
| |____httpd-dav.conf
| |____httpd-default.conf
| |____httpd-info.conf
| |____httpd-languages.conf
| |____httpd-manual.conf
| |____httpd-mpm.conf
| |____httpd-multilang-errordoc.conf
| |____httpd-ssl.conf
| |____httpd-userdir.conf
| |____httpd-vhosts.conf
|____httpd.conf
|____magic
|____mime.types
|____original
| |____extra
| | |____httpd-autoindex.conf
| | |____httpd-dav.conf
| | |____httpd-default.conf
| | |____httpd-info.conf
| | |____httpd-languages.conf
| | |____httpd-manual.conf
| | |____httpd-mpm.conf
| | |____httpd-multilang-errordoc.conf
| | |____httpd-ssl.conf
| | |____httpd-userdir.conf
| | |____httpd-vhosts.conf
| |____httpd.conf
|____other
| |____bonjour.conf
| |____php5.conf
|____users
{% endcodeblock %}
写一个alias到~/.bash_profile里，就更方便了:  
{% codeblock %}
alias tree="find . -print | sed -e 's;[^/]*/;|____;g;s;____|; |;g'"
{% endcodeblock %}
###Update
现在正在用的是 `homebrew` 安装的 `tree` 命令行。  
```
brew install tree
```
作者: Volcano 发表于November 22, 2010 at 8:34 pm  
版权信息: 可以任意转载, 转载时请务必以超链接形式标明文章原始出处和作者信息及此声明  
永久链接 - [http://www.ooso.net/archives/564](http://www.ooso.net/archives/564)  
