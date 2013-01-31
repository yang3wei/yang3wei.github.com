---
layout: post
title: "Octopress 新增博文时要注意的地方"
date: 2013-01-28 22:11
comments: true
categories: octopress相关
tags: [octopress]
---
用 MacVim 的请注意，执行 rake deploy 的时候，  
切记要将 MacVim 正在编辑的 markdown 博文文件保存并关闭掉~  
因为在 deploy 的时候会对 source/\_posts 目录下的文件执行 cp 操作~  
如果 MacVim 当前正在编辑博文的 markdown 文件的话，  
很有可能在执行 rake deploy 的时候给予失败的提示~  
究其根源就是因为 MacVim 锁定了某个文件（swp 格式的），  
然后对 rake deploy 命令的执行造成了阻碍~  
<!-- more -->  
不知道其他的版本的 vim（vim、gvim，etc） 会不会也受到这个问题的影响~  
我在 google 上面搜寻了一下这个问题，没有找到什么有用的资料~  
能够解决掉这个问题还是占了很大的运气成分的，  
刚开始我还误以为是 markdown、md 后缀带来的这个问题~  
因为在我将 source/\_posts 目录下的所有博文文件后缀由 markdown 改为 md 以后，  
发现又能正常的执行 rake deploy 命令了~  

关于 swp 文件
-
1.swp 文件是 vim 防止终端崩溃后恢复文件用的。  
每次当文件内容被修改而没有用 :w 保存的时候，都会有这么一个文件，  
这样可以解决多个用户编辑同一个文件，或忘记存盘而先关了终端的情况  

2.兄弟，那可是一个好东东啊，你还嫌它？  
我在 solaris 中误删了刚写的 shell 脚本，只有哭的份！  

octopress 发布博文的常规流程
-
作为一个过来人，我觉得这是一个非常有必要记录一下的问题！  
发布第一篇博文时该怎么弄我是咨询过 google 的~  
我最初的认识是这样的：  
在 Terminal 里面，通过 <code>cd ~/octopress</code> 命令切换到 octopress 的默认目录~  
然后，执行 <code>rake new\_post["博文标题"]</code> 命令，  
它的作用是在 ~/octopress/source/\_posts 目录下新建一个 “日期\_时间\_博文标题.markdown” 文件~  
接下来，通过 MacVim 等编辑器按照 markdown 的语法在上述新建的文件中撰写博文~  
撰写完毕之后再依次执行 <code>rake generate</code>、<code>rake preview</code>、<code>rake deploy</code> 命令  
它们的作用分别是：  
由 markdown 文件生成静态 html 页面，  
开启本地服务器提供预览（访问 <code>http://localhost:4000</code>），  
将静态页面部署到 github 博客服务器供他人访问。  
其中，rake preview 命令是否执行是可选的，  
如果你足够自信的话，完全可以跳过这一步直接进行部署~  

这种流程是没有问题的，不过效率比较低！  
假如在 \_posts 目录中有 800 个 markdown 文件，  
那么在执行 rake generate 命令时，会重新解析所有的 markdown 文件~  
这是对生命和电能的巨大浪费，可以使用如下步骤来提升效率：  
在撰写博文伊始就在 Terminal 中执行 rake preview 命令开启本地预览功能，  
它会实时监控 \_posts 目录，你的新增、保存动作都会被捕捉到，  
最最重要的一点是，你每执行一次保存操作，更新的内容都能在浏览器中有所反映，  
待工作完毕之后，你只需要部署、提交源文件一次就行了，大大地提高了撰写博文的效率！

twitter 被屏蔽导致持续请求的问题
-
国内用 octopress 架设好博客以后，  
因为天朝屏蔽了 twitter，而 octopress 又默认启用了 twitter，  
所以会导致一个问题（我用的是 mac 平台的 chrome 浏览器）：  
博客页面打开时，标签页里面的圆圈要转动很久才停止，  
浏览器左下角呈现字样：  
__Waiting for platform.twitter.com...__  

怎么解决这个问题呢？  
打开 octopress 所在的目录，编辑 \_config.yml 文件，  
将 <code>twitter_follow_button</code> 和 <code>twitter_tweet_button</code> 的值设置为 false 即可！

谈一下自己对 markdown 格式的感受：
-
markdown 格式用来写博文真的挺好的，没有糊里花哨的颜色料理，  
可以很好的区分开代码和文字，加粗样式什么的用起来也挺方便灵活的~  
给我的感觉就是：方便实用，精炼简洁，没有一丝冗余之感~  
另外，我非常喜欢 octopress 的默认主题，简洁而大气，  
我想我应该不会为了避免撞衫而去寻找其他的主题了，有了这个我就已经感到非常满足了~  
此刻我用 MacVim 写着 markdown 格式的博文，这感觉真是爽透了~  
从未曾想象此刻能如此轻松惬意地书写着博文，这美妙的初体验我将铭记于心！  
最后的最后，此种方式的博文书写，本地机器和 github 服务器人手一份拷贝，  
再也不用担心数据丢失的问题了~  

后记：  
-
历经磨难终于在 github 上面架好了自己的 blog，灰常开心啊~

