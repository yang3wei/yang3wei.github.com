---
layout: post
title: "设置 vim 默认编码为 utf-8"
date: 2013-01-28 14:00
comments: true
categories: Vim相关
tags: [vim]
---

花了不少功夫来学习 vim，这不终于有机会用 vim 来写博文了  
结果发现 vim 里面对编码格式的支持出了问题，  
输入的中文在再次打开的时候都玩变形记，- -、  
不过肯定是有解决办法的，谁让 vim 这么强大呢！  
遂 googling，即刻便找到了解决方案：  
<!-- more -->
<pre><code>" =====================
" 多语言环境
" 默认为 UTF-8 编码
" =====================
if has("multi_byte")
    set encoding=utf-8
    " English messages only
    "language messages zh_CN.utf-8
  
    if has('win32')
        language english
        let &termencoding=&encoding
    endif
  
    set fencs=ucs-bom,utf-8,gbk,cp936,latin1
    set formatoptions+=mM
    set nobomb " 不使用 Unicode 签名
  
    if v:lang =~? '^\(zh\)\|\(ja\)\|\(ko\)'
        set ambiwidth=double
    endif
else
    echoerr "Sorry, this version of (g)vim was not compiled with +multi_byte"
endif</code></pre>

将上面的配置追加到 ~/.vimrc 文件中  

PS:  
发现一个写 markdown 的时候要注意的地方，  
在 categories: 后面追加类别的时候需要用空格隔开，  
应该是做的强制性要求，不然在用 rake generate 命令生成的时候会出错：  
<pre><code>ERROR: YOUR SITE COULD NOT BE BUILT:
(&lt;unknown&gt;): could not find expected ':' while scanning a simple key at line 6 column 1 in /Users/user/octopress/source/_posts/2013-01-28-she-zhi-vim-mo-ren-bian-ma-wei-utf-8.markdown</code></pre>

不过这样也好，用空格隔开的话，看起来显得更加泾渭分明~
