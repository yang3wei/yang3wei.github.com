---
layout: post
title: "解读很强很彪悍的 Python 标准 GUI 库"
date: 2013-02-09 21:25
comments: true
categories: Python相关
tags: [python]
---
原文链接：[解读很强很彪悍的Python标准GUI库](http://developer.51cto.com/art/201002/184975.htm)  
Python 标准 GUI 库的创始人为 Guido van Rossum。在一个不经意间的时刻决心开发一个新的脚本解释程序，那也就是 Python 语言。
<!-- more -->
Python 可以使用户避免过分的语法的羁绊而将精力主要集中到所要实现的程序任务上，GUI 开发方面，既有 Python 标准 GUI 库——TKinter，又有很多强大的第三方 GUI 库，例如 Python 标准 GUI 库。

###1. Guido 简介
Python 创始人，原居荷兰，1995 年移居美国，2005 年加入 Google。

###2. 为什么叫 Python？ 
说来很有趣，选用 Python 这个名字，仅仅是因为 Guido 很喜欢一部叫做《Monty Python 飞行马戏团》的肥皂剧。

###3. Python 是什么？ 
是一门可以被应用到很多领域、功能强大、面向对象、跨平台的动态编程语言。1990年至今，Python 经过 17 年的发展，已经成为最流行的编程语言之一。在 Google，Python 语言更是被广泛应用，想在 Google 工作，Python 语言似乎成了一个基本要求。

Python何以有这么大的魅力，受到如此的追捧？笔者结合自己的使用经验，认为 Python 的强大，主要体现在以下几个方面：

###一、简单易学 
Python 世界非常强调 “简单” 二字，一个代码风格良好的 Python 程序，阅读起来，感觉就像是在阅读一段英文。Python 的这种伪代码本质，使得你可以更关注如何解决实际问题，而不是关注语言本身。

Python 的语法也相当简单，并且内置了很多高级数据结构。 Python 的简单易学，很适合作为入门语言。目前，包括麻省理工学院在内的国外很多高校，都已选用 Python 作为教学语言。

###二、代码量小 
实现同样的功能，Python 标准 GUI 库与 Java、C# 这样的“大个头”比起来，明显简约很多。 例如，打印出一个文本文件中的所有内容，用 Python 只需要一句。

如果你仅仅认为用 Python 只能写写 “Hello World”，那你就大错特错了。 Python 可以被应用到网络开发、GUI 开发、图形开发、Web 开发、游戏开发、手机开发、数据库开发等众多领域。

网络开发方面，Python 提供了大量可用的网络编程模块，涉及到 Socket、EMail、FTP 等等；众所周知的豆瓣网（http://www.douban.com/），就是使用了专门用于 Python 的网络开发框架——Twisted；

此外，Python 还支持 Jabber 等等。 GUI 开发方面，既有 Python 标准 GUI 库 —— TKinter，又有很多强大的第三方 GUI 库，例如 Python 标准 GUI 库。Web 开发方面，Python 更显强大。应用服务器，有 zope；CMS 系统，有 plone（基于 zope）。国内，润普科技（http://zopen.cn/），就是做基于 plone 应用的；此外，还有 django —— 一个可以和 RoR 相媲美的快速 web 开发框架、Pylons 等等。 游戏开发方面，Python 也有举足轻重的地位。很多网络游戏脚本，例如账号注册系统、物品交换系统、场地转换系统和攻击防御系统，都是用 Python 写的，与 C++ 相比，Python 更加轻便。
