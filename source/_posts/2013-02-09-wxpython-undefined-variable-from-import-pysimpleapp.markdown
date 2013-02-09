---
layout: post
title: "wxPython Undefined variable from import PySimpleApp"
date: 2013-02-09 23:41
comments: true
categories: Python相关
tags: [python, eclipse]
---
在 python 里面，将文本文件的内容读取为字符串只需要一行代码：<!-- more -->
{% codeblock %}
string = open('/Users/user/Desktop/Test.txt').read()
{% endcodeblock %}
当然，这还只是 python 众多亮点中的冰山一角。尽管使用 java 已经有好几个年头，足以解决日常开发中遇到的一些小问题了，但我还是希望能拓宽一下视野，掌握一门新的语言。python 简洁实用，难度曲线感觉也不是很陡，所以就成了我的首选。在我的观念中，命令行程序虽然也能做地很强大，但作为方便日常开发的小工具，我还是希望能有一个合体的 GUI 壳子套在它的外面。经过多番查探，我找到了 python 几个比较主流的图形库：TKinter、wxPython、JPython。其中，TKinter 好像是 python 内置的，wxPython 需要自己下载安装包执行安装，JPython 不是很了解，但是在 eclipse 的 Preferences 里看到过它的身影，不知道能不能直接拿来用。

####TKinter 的 HelloWorld 比较简单，下面给一个创建窗口的程序例子：  
{% codeblock TestTKinter.py lang:python %}
#!/usr/bin/python
# -*- coding:utf-8 -*-

import Tkinter
top = Tkinter.Tk()
# Code to add widgets will go here...
top.mainloop()
{% endcodeblock %}

####wxPython 的 HelloWorld 如下：  
{% codeblock TestWxPython.py lang:python %}
#!/usr/bin/python
# -*- coding:utf-8 -*-

import wx

if __name__ == '__main__':  
    app = wx.PySimpleApp()
    frame = wx.Frame(parent=None)
    frame.Show(True)
app.MainLoop()
{% endcodeblock %}

不过，在试验 wxPython 的 HelloWorld 之前还得经历下载、安装和配置的步骤。就拿我此次的经历来说吧，下载倒是没遇到什么问题，我的操作系统是 Mac OS X Lion 10.7.2，Python 的版本是 2.7.1，在官网下载了相对应的一个版本：wxPython2.8-osx-unicode-2.8.12.1-universal-py2.7。因为之前也有对 python 的粗浅涉猎，所以在 eclipse 里面早早地就把 PyDev 给装上去了，具体怎么弄地现在也想不起来了，应该不是很麻烦。安装也没什么好说的，一路 next 就是了。至于配置，其实不弄好的话对 wxPython HelloWorld 的执行也没有致命的影响。

不过有一点儿非常不好，你不给配置的话，在  
{% codeblock %}
app = wx.PySimpleApp()
frame = wx.Frame(parent=None)
{% endcodeblock %}
这两行代码的前面就老喜欢显示红叉叉。  
挺蛋疼的，执行的话又没有影响，非得弄俩红叉恶心你！

那么，怎么终结这种不利的情况呢？  
Googling 之后找到了一个很有用的链接：  
[是配置问题吗？为什么IDE显示undefined variable from import:xxx](http://bbs.csdn.net/topics/340237664)  
虽然我的平台是 Mac OS，但从帖子中给出的最终解决方案来看，应该也就是找到 wxPython 库相关的路径加到 `PyDev` -> `Interpretor - Python` -> `Libraries` 里面。这个路径在哪儿一时半会儿我也是一头雾水，再加上 wxPython 是否被成功添加到 python 的 System Libs 要在重启 eclipse 之后才能确认（实际表现为配置完毕之后不会立即生效，要在重启 eclipse 以后红叉才会被清除掉），所以还是费了我一些功夫的，这里给出路径：  
{% codeblock %}
/usr/local/lib/wxPython-unicode-2.8.12.1/lib/python2.7/site-packages/wx-2.8-mac-unicode
{% endcodeblock %}



