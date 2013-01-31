---
layout: post
title: "MacVim 用 mvim 命令打开文件到新的标签页"
date: 2013-01-30 15:22
comments: true
categories: vim相关
tags: [vim]
---
原文链接：  
[http://www.reake.com/how-to-use-terminal-command-to-open-file-to-macvim-mvim-tab/](http://www.reake.com/how-to-use-terminal-command-to-open-file-to-macvim-mvim-tab/ "瑞克互动")  
从 `MacVim(GitHub)` 官网下载后，解压出两个文件：`MacVim.app` 和 `mvim`  
<pre><code># 将 `MacVim.app` 拷入 `/Applications` 目录
sudo cp -f MacVim.app /Applications/
# 将 `mvim` 拷入 `/usr/bin` 目录
sudo cp -f mvim /usr/bin/</code></pre>

然后在 `Terminal` 键入命令 `mvim project_file.php` ，出现了一个 MacVim 窗口。  
但 `MacVim` 支持当前窗口多标签页功能，每次打开都是新窗口，  
虽然苹果有 `Mission Control` 切换，但文件窗口多了，显示还是很麻烦，  
所以想让 `mvim` 打开文件直接在 `MacVim` 当前窗口的新标签页里打开，  
需要在命令后加 `--remote-tab` 参数，感觉挺麻烦，何不设置为默认就在标签页中打开呢?    
<!-- more -->
这里给出一种直接修改 `mvim` 以达到上述目的的方案：  
1.切换到 `/usr/bin/` 目录并打开 `mvim` 文件
<pre><code>cd /usr/bin/; mvim mvim</code></pre>
2.更改 `/usr/bin/mvim/` 文件中的配置
首先，在文件头部加入
<pre><code>tabs=true</code></pre>
然后，把底部的 `if` 块替换成下面的：
<pre><code>if [ "$gui" ]; then
  if $tabs && [[ `$binary --serverlist` = "VIM" ]]; then
    exec "$binary" -g $opts --remote-tab-silent ${1:+"$@"}
  else
    exec "$binary" -g $opts ${1:+"$@"}
  fi
else
  exec "$binary" $opts ${1:+"$@"}
fi</code></pre>


