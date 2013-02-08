---
layout: post
title: "为 Eclipse for Mac 新增 Show in Finder 的功能"
date: 2013-02-08 22:47
comments: true
categories: Mac相关
tags: [mac, java]
---
相信各位使用 Mac OS 作为开发平台的同僚都遇到过如下的问题：  
想在 Finder 中显示 eclipse 工程中的某个文件，挺不容易的！  
不知道其他人是怎么弄的，这里说一下我是怎么弄的吧：  
<!-- more -->
__1.在工程视图里选中要在 Finder 中显示的文件__  
__2.在弹出的右键菜单中选择 Properties__  
__3.复制该文件父目录的绝对路径__  
__4.打开 Finder__  
__5.Command + Shift + G 弹出快速前往的对话框__  
__6.在路径输入框中粘贴之前复制的父目录绝对路径__  
__7.回车前往目标文件所在的父目录__  
__8.手动在 Finder 的父目录中定位目标文件__  

不是很难，但由上面的步骤可以看出，操作起来还是挺麻烦的。  
一次两次可能还不觉得，但如果是经常要执行类似的操作就很烦了~  
我就一直在为这个问题而感到烦恼！如何来终结这种不利的境况？  

##这里介绍一个解决方案
为 eclipse 扩展一个自定义的功能！  
__原理很简单，就是以命令行的方式来调用外部的可执行文件来满足自己的需求。__  

再对上面的需求做一下总结吧：  
不就是要在 Finder（Mac OS）或者 Explorer（Windows）里面定位出某个文件么，  
从编写程序的角度来看，文件的绝对路径都能拿到了，定位到该文件还有什么好难的？  

摸索过程中我找到了一篇很有用的资料：  
[In eclipse, reveal current file in filesystem](http://stackoverflow.com/questions/1161240/in-eclipse-reveal-current-file-in-filesystem)   

浓缩的精华：  
__You can also develop your own external tool to open the file in a Windows explorer__  
{% img center /images/posts/meaningful.gif %}

到这里就算是有头绪了，如图所示：  
仅需对上图中针对 `Windows` 的配置做更改，适配到 `Mac` 平台就行了。  
其实很简单，改几个地方就 ok 了：  
第一个是 `Name`，改成你喜欢的名字吧，比如说：`show in finder`  
第二个是 `Location`，改成要调用的外部可执行文件的完整路径即可，如：`/usr/bin/open`  
第三个是 `Arguments`，涵义是要给外部可执行文件传入的参数。  
这里可以再次借助 eclipse 提供的功能来简化我们所要做的操作：  
${resource_loc} 可以拿到当前所选择文件的绝对路径；  
${container_loc} 可以拿到当前所选择文件父目录的绝对路径。  
{% img center /images/posts/external_tools_configurations.png %}
{% img center /images/posts/external_tools_configring.png %}
`/usr/bin/open` 是 `Mac` 的内置命令，  
`open` 命令带 `-R` 参数表示在 `Finder` 中呈现出目标文件，  
（注：不带 `-R` 参数的话就会选择相关联的程序来打开该文件）  
这样一来的话，就算是大功告成了~  

##如何来执行这个扩展出来的功能呢？
也很简单，不过先要将名为 `show in finder` 的扩展功能设置为 `favourite`：  
在 `Organize Favourites...` 里面 `add` 一下即可！  
{% img center /images/posts/set_as_favourite.png %}
接下来就开始享受便利的生活吧：  
首先，选择要在 `Finder` 中显示的目标文件，  
然后，按一下工具栏 `Run` 按钮右边和 `Run` 按钮长的比较像的那个按钮。  



 

