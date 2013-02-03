---
layout: post
title: "Octopress 代码高亮"
date: 2013-02-03 17:43
comments: true
categories: Octopress相关
tags: [octopress]
---
原文链接：  
[http://xiongbupt.github.com/blog/2012/06/08/octopressdai-ma-gao-liang/](http://xiongbupt.github.com/blog/2012/06/08/octopressdai-ma-gao-liang/)  
现在博客已经基本搭建完毕，下面对从`jekyll bootstrap`搬迁到做一个基本介绍。首先是代码高亮部分，下面的文章主要来自于[Octopress关于代码高亮部分][lab1]。文章主要是对上面的内容进行一个简单的翻译，以及简单介绍从`jekyll bootstrap`上面的代码移动到`Octopress`上面做一个简单的介绍。
<!-- more -->
##共享代码片段
在博客上共享代码片段应该是简单的，并且代码应该具有简单的高亮功能。Octopress也具有这种功能，在`jekyll`上，其使用的是`pygment`来实现代码高亮的功能，Octopress实现的更好。在Octopress上面有下面几种选择：   

* 使用[Solarized高亮][lab2]主题来实现（该方法未尝试，实际上不知道怎么弄，只是凭借个人的猜测）。
* 使用Gist的代码内嵌。
* 从自己的文件系统中插入代码片段，该代码具有下载链接。
* 简单的内嵌代码块带有`<figure>`和`<figcaption>`和可选的下载链接。
* Pygments caching（似乎还是使用Pygnment来实现代码高亮）。
* 通过javascript脚本使得代码具有行号。

###Solarized高亮
这部分内容自己没有尝试，待更新。

###反引号的代码块
同时使用`backtick_codelock`过滤器，可以使用Github的最适用的代码高亮块。使用三个反引号开始，后面跟着一个空格，接下来是高亮语言，具体语法如下所示：
      ``` [language] [title] [url] [link text]
      code snippet
      ```
按照上述方式便可以对相应的代码块进行高亮，如下面的例子。
####example1（原文)
     ```
     $sudo give me a hug
     ```
按照上面的方式在文本中输入之后，产生的效果如下：
```
$sudo give me a hug
```
####example2带有说明和连接
     ``` ruby Discover if a number is prime http://www.noulakaz.net/weblog/2007/03/18/a-regular-expression-to-check-for-prime-numbers/ Source Article
     class Fixnum
       def prime?
         ('1' * self) !~ /^1?$|^(11+?)\1+$/
       end
     end
     ```
产生的代码片段如下：
``` ruby Discover if a number is prime http://www.noulakaz.net/weblog/2007/03/18/a-regular-expression-to-check-for-prime-numbers/ Source Article
class Fixnum
  def prime?
    ('1' * self) !~ /^1?$|^(11+?)\1+$/
  end
end
```

###Gist代码内嵌
当使用这种方式的代码高亮时，仅仅需要的是gist的id，对gist不了解的可以上google搜索下，个人的理解是，gist对每段用户上传的代码段都会有一个对应的id，当用户给出对应的代码的id后，将会从gist上面下载对应的已经高亮的html文件，最终在用户的页面上显示出来。
####语法
     { % gist gist_id [filename] %}
####example
     { % gist 996818 %}
上述代码的片段在Octopress中的markdown文件中输入之后，得到的效果如下：  

{% gist 996818 %}

如果一个gist的id对应有多个文件，这时需要对想要高亮的文件添加文件名，具体语法如下所示：   
      { % gist 1059334 svg_bullets.rb %}
      { % gist 1059334 usage.scss %}

总体来说，gist代码高亮是很简单的，只是需要将代码上传到[gist][lab3]，然后获取相应的id然后按照上面的语法进行设置即可。只是每次在写博客时，都需要对博客文章中的代码拷贝到网址上生成，在没有网时，代码高亮比较麻烦。

###从本地文件中引入代码
这种方式在Arch上面有个小问题，在Arch上的报错如下所示：  
``` bash Arch error
Building site: source -> public
  File "<string>", line 1
    import sys; print sys.executable
                        ^
SyntaxError: invalid syntax
```
上网搜索之后在[该网址][lab4]发现解决方法。   
这种方式需要使用python2来完成代码高亮的操作，由于Arch的python指向的是python3，而python3无法完成该功能，因此需要在`plugin`文件夹中再添加一个新文件，指定在`Octopress`运行时使用的是python2，具体增加的文件内容如下所示，文件的名字为`ruby_python_arch_linux_fix.rb`  
{% include_code Set the ruby to run python2.7 ruby/ruby_python_arch_linux_fix.rb %}

在完成该操作之后，仍然报错，得到如下错误：  
``` bash Error message
LoadError: Could not open library 'lib.so': lib.so: cannot open shared object file: No such file or directory
```
参考[该网址][lab5]的由`elidos`提出的解决方法可以知道是`rubypython`自身的bug，需要修复，具体修改文件在Arch上面为`/usr/lib/ruby/gems/1.9.1/gems/rubypython-0.5.3/lib/rubypython/pythonexec.rb`，修改位置大概在126行左右，修改后的内容如下所示：
```diff fix the rubypython bug
--126 %x(#{@python} -c "#{command}").chomp if @python
++126 %x("#{@python} -c #{command}").chomp if @python
```
完成该修改之后，便可以使用这种包含代码的方式来进行代码高亮。   
使用这种方式的语法也参见[octopress手册][lab1]，此处不再进行详细说明，这种方式的代码是单独存放成一个文件保存在本地系统下，当代码长度较长，又不想放在博客的正文中时，使用该方法比较好。  
简单记录下这种方式的语法如下所示：（目前不知道为什么，在octopress中输入\{ %的方式都会进行代码解析，所以上面的\{ %都进行了添加了` `来取消，在实际文章中输入的时候，将` `取消掉）
    { % include_code [title] [lang:language] path/to/file %}
其中的中括号的内容是可选的，具体语言便是这种方式，当需要强制高亮时，需要指定`lang:`这个参数，很好用。
###使用Code Block的方式
目前自己的博客这种方式用的比较多，前面写的文章目前全部修改成为了这种方式，感觉这种方式和`pygnment`的方式差不多，之前全部采用的是`pygnment`的方式，利用正则表达式把所有文章的代码高亮全部改成了使用`code block`。它的具体语法如下所示：（与`pygnment`很相似，指定语言即可）
      { % codeblock [title] [lang:language] [url] [link text] %}
      code snippet
      { % endcodeblock %}
和之前描述的类似，中括号的内容是可选的。
####使用正则表达式替换
这种方式可以替换博客内容，使得博客中所有文章的代码高亮使用`Code Block`。自己使用的是`sed`来完成的操作，似乎都没有用到正则表达式，只是简单的替换，做个简单的记录吧。
#####替换方法
进入到`source/_posts`目录下，在终端输入如下代码：
``` bash Sed查看文章代码
sed -n 's/\(\ highlight\ \)/\1/p' *
```
上面将会把当前目录下的含有\{\% highlight的文件的那行都显示出来，中间会将含有该特殊字符的行都打印出来，中间可以看到自己的博客内容都用到了哪些类型文件的语法高亮，将对应的代码高亮进行替换即可。  
其实这种方式就是简单的搜索替换，应该算不上使用正则表达式了，只是写下来做下笔记了，防止以后再用具体操作如下所示：
``` bash Sed替换文件内容
sed -i 's/\(\ highlight\ bash %}\)/\ codeblock\ lang:bash\ %}/' *
```
[lab1]:http://octopress.org/docs/blogging/code/ "Octopress代码高亮"
[lab2]:http://ethanschoonover.com/solarized "Solarized高亮"
[lab3]:https://gist.github.com/
[lab4]:http://blog.gonzih.org/blog/2011/09/21/fix-octopress-pygments-error-on-arch-linux/
[lab5]:https://github.com/tmm1/pygments.rb/issues/10




