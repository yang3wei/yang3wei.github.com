---
layout: post
title: "Java -Dfile.encoding=UTF-8 干掉乱码"
date: 2013-02-10 15:18
comments: true
categories: Java相关
tags: [java]
---
参考链接：  
[java 乱码问题-Dfile.encoding=UTF-8](http://hi.baidu.com/cnjsp/item/a2c9c9aed1de96a829ce9d24)  
[Java's file.encoding property on Windows platform](http://yaojingguo.blogspot.com/2009/02/javas-fileencoding-property-on-windows.html)  
[How do you open a jar file on a mac?](http://wiki.answers.com/Q/How_do_you_open_a_jar_file_on_a_mac)  
<!-- more -->
###遭遇乱码问题的来龙去脉
这两天写了一个 Java 程序来玩，结果又遭遇了以前遇到过很多次的乱码问题，具体描述一下：  
在 Mac 系统里面，常用的 Java 程序的启动方式有如下几种：  
__1.通过 eclipse 执行 class 入口文件启动；__  
__2.在 Terminal 里面用 java Test.class 或 jave -jar Test.jar 启动__  
__3.通过 ant 执行 class 入口文件启动；__  
__4.直接用 ant 执行 jar 文件；__  
__5.用 Mac OS CoreServices 中的 Jar Launcher.app 执行 jar 文件。__  
__6.用 Mac OS 自带的 Jar Bundler.app 将 jar 文件包装成 app，然后执行__  

执行途径还是相当地丰富，但以不同的方式来执行，从控制台中得到的程序输出也不一致  
比如说，刚刚在 `eclipse` 中还能正常打印出来的汉字，在打成 `jar` 包以后，  
双击该 `jar` 文件以 `Jar Launcher.app` 的方式来启动，打印出来的文字就成了乱码了。  
毕竟写出来的 `java` 程序最终还是要打成 `Jar` 包来使用的，总不能每次都在 `eclipse` 中启动吧？  
前面说过，不是第一次碰到这种问题了，于是便想着要把这个问题给解决下。  
灵机一动之下想到一个好办法，在这些启动方式下均把 `System` 中的属性遍历打印出来，  
然后用 `git` 来做各个版本的差异比较，有可能会套出一些蛛丝马迹~  
抱着试一试的想法实践了一把，果然发现一些猫腻，集中体现在 `file.encoding` 这个属性上面。  
在 `file.encoding` 属性的值是 `UTF-8` 时，是不存在乱码问题的，`eclipse` 执行就属于这种情况。  
`Jar Launcher.app` 执行时，该属性的值就变成 `MacRoman` 了，  
上面给出的资料中有对该属性的介绍，可以用 `java -D<name>=<value> Test.jar` 来更改它。  
另外，只有在启动 `java` 程序前通过传递参数来更改才有效，程序一经启动就无法再更改了。  
这样的话，也就只有通过传递 jvm 参数的方式来做默认编码的变更了：  
__其一，写一个带 `-Dfile.encoding=UTF-8` 参数的脚本文件来启动；__  
__其二，用 `Jar Bundler.app` 打包成 `app`，效率应该不如第一种方案。__  
原理其实都差不多，都只是将更改 `jvm` 默认编码的操作封装了起来，执行时就不用再手动键入了。  

###java 乱码问题 -Dfile.encoding=UTF-8
`-Dfile.encoding` 解释：  
在命令行中输入 java，在给出的提示中会出现 -D 的说明：  
`-D<name>=<value>`  # set a system property  
-D 后面需要跟一个键值对，作用是设置一项系统属性  
对 `-Dfile.encoding=UTF-8` 来说就是设置系统属性 `file.encoding` 为 `UTF-8`  
那么 `file.encoding` 什么意思？字面意思为文件编码。  
搜索 java 源码，只能找到 4 个文件中包含 `file.encoding` 的文件，  
也就是说，只有四个文件调用了 `file.encoding` 这个属性。  
在 `java.nio.charset` 包中的 `Charset.java` 中，这段话的意思说的很明确了。  
简单说就是默认字符集是在 java 虚拟机启动时决定的，  
依赖于 java 虚拟机所在的操作系统的区域以及字符集。  
代码中可以看到，默认字符集就是从 `file.encoding` 这个属性中获取的。  

###Java's file.encoding property on Windows platform
This property is used for the default encoding in Java, all readers and writers would default to use this property. "file.encoding" is set to the default locale of Windows operationg system since Java 1.4.2. System.getProperty("file.encoding") can be used to access this property. Code such as System.setProperty("file.encoding", "UTF-8") can be used to change this property. However, the default encoding can not be changed dynamically even this property can be changed. So the conclusion is that the default encoding can't be changed after JVM starts. "java -Dfile.encoding=UTF-8" can be used to set the default encoding when starting a JVM. I have searched for this option Java official documentation. But I can't find it.

###How do you open a jar file on a mac?
You can indeed launch a jar file from the command line, with the following command:   
`java -jar yourfile.jar`  
As well as this you can assign "Jar Launcher" as the default app. To use when you double-click a jar file, as follows (I don't believe you need the developer tools installed for this):   
Click once on the .jar file in the Finder and then from the menubar in the Finder select File -> Get Info". Click on "Open with" and from the popup menu select "Other". A file browser window will open. In this window, go to the `/System/Library/CoreServices` folder and select 'Jar Launcher'. Then make sure the "Always Open With" checkbox is checked and then click Add. Then click the "Change all" button so that any jar file will be opened automatically. Finally, close the Info window and now when you double-click any of your jar files they should run automatically.
