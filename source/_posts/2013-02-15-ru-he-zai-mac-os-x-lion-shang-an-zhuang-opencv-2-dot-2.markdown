---
layout: post
title: "如何在 Mac OS X Lion 上安装 OpenCV 2.2"
date: 2013-02-15 10:19
comments: true
categories: OpenCV相关
tags: [OpenCV, C/C++]
---
原文链接：[如何在Mac OS X Lion上安装OpenCV 2.2! ](http://blog.sina.com.cn/s/blog_677a6d640100xcx9.html)  
OpenCV 对于做计算机视觉的童鞋来说就想老鼠与大米的关系，总之一个词——重要！  
<!-- more -->
现在网上很多的帖子都是教如何在 VS2008、VS2010 下配置 OpenCV 2.x 的，这里就不再赘述。本帖主要解决的问题是介绍如何在 Mac OS X Lion（10.7.2) 下安装 OpenCV 2.2。其实本帖所描述的方法并不是原创的，最原始的帖子请参考[这里](http://salemsayed.me/?p=240)（英文）。  

##准备
首先你需要下载并安装好 [MacPorts](https://distfiles.macports.org/MacPorts/MacPorts-2.0.3-10.7-Lion.dmg) 这一大神级的软件。如果没有听说过 MacPorts，请 Google 或者上[这里](http://en.wikipedia.org/wiki/Macports)。注意要下载 Lion 版的，这个跟 Snow Leopard 版的安装文件不一样，本文中的链接即是。  
安装好了以后，进入 Terminal，输入以下两条命令，这是保证你的 MacPorts 是最新的。  
```
sudo port selfupdate
sudo port upgrade outdated
```
至此，准备工作完毕，下面开始正式安装。（不要关掉 Terminal ！）  

##安装OpenCV
可能有童鞋会问问什么到现在都没有讲到哪里下载 OpenCV？有这个问题就说明你不知道 MacPorts 为啥是大神级的工具啦。MacPorts 是一个维护了 8300 多个（还在增长）开源软件的 port，它能够自动地帮你下载、安装、配置好一款软件。你所要做的仅仅是输入一条命令，这里，我们只需要输入：  
```
sudo port install opencv
```
然后呢？然后就是漫长的等待，你可以去做其他事情，但请不要碰你的 Mac，Terminal 上会列出一串串下载列表，机器会比较热。  
大约。。。一个多小时后，当 Terminal 里的提示符再次出现时，你的 OpenCV 就安装好了。当前 OpenCV 的最新版本是 2.3.1，但 MacPorts 的数据库里记住的版本还是 2.2，估计这里还有一个延时吧。  

##XCode 中配置 OpenCV
现在，你可以启动你的 XCode4 了。创建一个 CommandLine Tool 项目。在写程序之前，需要配置两个地方：  
1、在左侧栏里的 Targets 栏目下，选择右侧窗口中的 “All”，这样可以显示所有选项；然后搜索 “Headers search path”，在这个里面添加 “opt/local/include”这个 OpenCV 的包含文件目录；  
2、在左侧栏的 Project 栏目下，添加一个 New Group，随便起个名字，然后右击添加文件到这个 Group 中，在添加文件窗口中输入 “/” 弹出寻找目录窗口，输入 “/opt/local/lib”，打开 OpenCV 的库文件目录，选择一下几个文件： libopencv_ml.2.2.0.dylib, libopencv_highgui.2.2.0.dylib, libopencv_core.2.2.0.dylib，当然根据需要可以添加其他库文件。  

这样，你就可以写一些简单的利用 OpenCV 的程序了。  
