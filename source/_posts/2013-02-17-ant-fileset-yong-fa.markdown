---  
layout: post  
title: "Ant fileset 用法"  
date: 2013-02-17 07:53  
comments: true  
categories: Java相关  
tags: [java]  
---  
原文链接：[ant fileset用法](http://blog.csdn.net/ajaxbloger/article/details/2303841)    
fileset用来定义目录位置及操作适用于该目录下的那些子目录或文件<!-- more -->  
##1. 拷贝单个文件到指定目录下，例：  
```  
<copy todir="${basedir}/new" file="${basedir}/old/old1.txt1">   
```  
将 __${basedir}/old/old.txt__ 文件拷贝到 __${basedir}/new__ 下  
  
##2. 拷贝一批文件到指定目录下，例：  
```  
<copy todir="${basedir}/new">  
    <fileset dir="${basedir}/old">  
        <include name="old1.txt" />  
        <include name="old2.txt" />  
        <exclude name="old3.txt" />  
    </fileset>  
</copy>  
```  
这里 fileset 定义的是原文件的组成形式，`<include/>` 子属性表示包括，`<exclude/>` 子属性表示排除，很简单，通过他们组合实现多文件的筛选，当然我这个例子用得很傻。比如  
```  
<include name="appgen/**"/>  
<include name="ibatis/**"/>  
<exclude name="**/*.log"/>  
```  
拷贝 `appgen` 目录和 `ibatis` 目录下除了 `.log` 文件以外的其它所有文件和子目录。  
可以把 `<fileset/>` 简写成 `<fileset dir="${basedir}/old" includes="old1.txt,old2.txt" />`，`includes` 可以理解成 `include` 的复数形式，包含多个文件时用逗号隔开，`excludes` 也一样。  
  
##3. 拷贝一个目录到指定目录下，例：  
```  
<copy todir="${basedir}/new">  
    <fileset dir="${basedir}/old">  
        <include name="appgen" />  
        <include name="appgen/" />  
        <include name=appgen/**" />  
        <include name="appgen/***" />  
    </fileset>  
</copy>  
```  
同样使用 `<fileset/>` 属性，`name` 指定目录名，不过这里要分两种情况，用 `<include/>` 子属性和不用 `<include/>` 子属性.  
若使用 `<include/>`， 又要分三种情况：  
若是 `appgen`，则只会拷贝名为 `appgen` 的空目录过去，它里面的文件和子目录则不会拷贝。  
若是 `appgen/`，或 `appgen/**`，则会把整个 `appgen` 目录拷贝过去，包括里面的文件和子目录。  
若是 `appgen/*`，则只会把该目录和该目录下第一级子目录的所有东西拷贝过去，而不会拷贝第二级和第二级以下的。  
__注： `appgen/*` 这儿是一个 * 号，* 号若大于两个，也跟一个 * 号是同样效果。比如 `appgen/*` 和 `appgen/****` 都只拷贝 `appgen` 目录下第一级子目录。__  
__注：若 `appgen` 这个目录本身就是个空目录(就是不存在)，无论怎么写，这个空目录都不会被拷贝。也就是说，`copy` 操作不会产生创建空目录的作用，要想创建空目录，只有用 `mkdir`。__  
若不使用任何 `<include>` 属性，如  
```  
<fileset dir="${basedir}/old">  
</fileset>  
```  
则会拷贝 __${basedir}/old__ 下的所有文件和子目录。  
__注：使用 `<exclude/>` 排除目录时，目录名必须写成 `appgen/` 或 `appgen/**` 形式，否则不会生效。__  
  
以上是三种拷贝到目录的种类，注意如果计算机中没有 `todir` 指定的路径，Ant将会自动创建这个路径。  
  
##4. 拷贝单个的文件：   
```
<copy tofile="old.txt" file="new.txt" />
```
就这么简单就行了。当然也可以写成：  
```
<copy tofile="${basedir}/new/new.txt">  
    <fileset dir="${basedir}/old" includes="old.txt" />  
</copy>  
```
这里 `includes` 就只能写一个文件，不能写上多个文件，因为不能将多个文件复制到一个文件中去，所以这样麻烦的写法是没有意义的。这个地方还有待去验证一下.  
  
复制肯定还要涉及到同名覆盖的问题，Ant 在 copy 类的 API 中说明：__Files are only copied if the source file is newer than the destination file__，这里的 newer 是指文件的修改时间，即使你在修改时文件内容没有任何变化，只是导致修改时间变了，Ant 同样会覆盖同名文件，也就是说，Ant不会检查文件内容。  
  
对于是复制目录的情况，由于目录没有修改时间，Ant 还是通过检查目录内文件的修改时间来决定是否覆盖的，若目录内某文件修改时间有变化，则会覆盖这个文件，而不是整个目录。  
  
如果要强行覆盖，`<copy/>` 有个 `overwrite` 属性，默认为 `false`，改成 `true` 就行了。  
Ant 真是太方便了，以前都没注意到它。功能很强大，能创建数据库，配置服务器，部署发布应用……只需要写好 `build.xml` 文件，剩下的就交给 Ant 来 “安装” 你的 WEB 应用了。  
以上就这些:  
您也可以去 apache Ant 项目里去看一下 fileset 的用法。  
