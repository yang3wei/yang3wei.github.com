---
layout: post
title: "I found a bug with CCParticleSystem"
date: 2013-02-08 01:41
comments: true
categories: Cocos2d相关
tags: [iOS, cocos2d]
---
此次经验对我影响很大，虽然只是一个很小的问题，  
但我还是没有掐掉上报 bug 的想法，于是在论坛上我碰到了一个很友善的老外。  
一念之间，我因为自己的一点小善心而获得了更多的知识，  
了解到了怎么上报 bug，怎么使用 git，还锻炼了英文，  
实乃是一举多得，总结一下，遇到问题还是不要犯懒，稍微较下真儿可能就会受益匪浅！
<!-- more -->
帖子链接：[I found a bug with CCParticleSystem.m](http://www.cocos2d-iphone.org/forum/topic/36963?replies=1#post-174889)  
发现了一个 cocos2d 粒子系统的bug，  
怎么说呢，也就是同一份由 Particle Designer 生成的 plist 文件，  
放到高低清的不同模式下，竟然得到了不同的视觉呈现。  

觉得诡异之余我仔细观察了一下，发现该问题可能是由于y方向的重力数据有异所致。  
于是我切到  `CCParticleSysem.m` 里面找了一番，发现由 plist 文件里面加载进来的数据，  
差不多都有做乘以  `CC_CONTENT_SCALE_FACTOR()` 的操作，  
但是 gravity 的 x 和 y 却没有。  

后来我将低清分辨率所使用的 plist 文件中的 `gravity-x`，`gravity-y` 数据都做了下除2处理，  
再一运行，发现问题已经不再，充分证明这就是问题的症结所在。  
看了下 cocos2d v1.01 和 v2.0.0 以及最新项目的 CHNAGELOG，  
都没有发现已经将这个 bug 修正的痕迹、记录，  
于是便在 cocos2d for iphone 主页申请了一个账号，将这个bug 贴到了论坛里面，  
希望有人能帮我转告 Riq，将这个 bug 修正掉~  

cocos2d 在我看来已经相当不错了，但是世界上很难有绝对完美的事物，  
经此一役，充分印证了这一点儿，一直以来都使用免费的 cocos2d 做游戏开发，  
现在终于能为 cocos2d 贡献出自己的一点儿微薄力量来了，甚感欣慰~  
