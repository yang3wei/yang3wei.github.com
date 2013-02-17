---
layout: post
title: "Ant 自动构建 build.xml"
date: 2013-02-05 13:10
comments: true
categories: Java相关
tags: [java, ant]
---
<!-- more -->
##功能：
生成的 `build.xml` 用来提供自动以 eclipse 执行，  
自动打 jar 包，自动以 ant 执行，自动执行生成的 jar 等功能~  

非常便捷的生成 java 工程的 `build.xml` ，  
`build.xml` 常用配置我给抽取到 `build.properties` 文件里面了。  
自此就可以尽可能少的去编辑、查看 `build.xml` 文件了  
（新增其他格式的资源文件要改动下）  
（第一次生成 build.xml 时会从 libs 目录中提取相关的数据）  
（生成完毕之后新添加的外部 jar 包也要在里面新增下）  

PathUtil 提供传入资源文件名称，返回完整包路径的便捷方法 getFullPath()，  
如，传入 `123.png`，返回 `/org/bruce/pocket/tools/res/123.png`  
这种格式的 “包路径” 能够很便利地被 java 程序使用，如：  
{% codeblock code snippet lang:java %}
InputStream is = 类名.class.getResourceAsStream("/org/bruce/pocket/tools/res/123.png");
byte[] bytes = IOUtils.toByteArray(is);
IOUtils.closeQuietly(is);
{% endcodeblock %}
这里得到的字节数组可以很方便的转换为字符串、图片对象什么的~  
转换为字符串：`google -》熔岩 51Blog-》StringFileBridge.java`  
转换为图片：`Image image = ImageIO.read(bytes);`  
##原理：
把 `org.bruce.pocket.tools.res` 包里面的  
`ant_template.properties` 、`ant_template.xml` 读取为字符串，  
做一些简单的文本替换，然后将新生成的字符串  
写入到 java 工程根目录的 `build.properties` 、`build.xml` 文件中，  

有一些必须要遵循的要求：  
资源文件必须放置在以 `.res` 结尾的 `package` 里面，  
对于 `PathUtil.java`，根据个人喜好来定义 DOMAIN（我的 domain 是 `org/bruce/`）  
生成的 `build.properties`、`build.xml` 也只是初步的，  
`build.properties` 里面的有些属性是必须要写上的，  
比如说，桌面的路径  
（这里涉及到将生成的 jar 文件复制到桌面的操作）  

还有就是应用程序的作者、当前版本、名称等，  
程序的名称我做了处理，直接拿工程文件的名称来用。  
版本、作者虽然不是重点，但规范的来说，  
最好还是写上，也免得 ant 执行打 jar 包操作的时候出问题。  
##上代码：
<!-- more -->
{% codeblock GenBuildXml.java lang:java %}
package org.bruce.pocket.tools.items;

import java.io.File;
import java.io.InputStream;
import java.util.List;

import org.apache.commons.io.IOUtils;
import org.bruce.pocket.tools.gui.Dta;
import org.bruce.pocket.tools.gui.GUI;
import org.bruce.pocket.tools.utils.IOBridge;
import org.bruce.pocket.tools.utils.PathUtil;

/**
 * @author Bruce Yang
 * 为 eclipse 的 java 工程自动生成 build.xml~
 */
public class GenBuildXml extends Dta {
	
	public GenBuildXml() {
		this.setTitle(this.getClass().getSimpleName());
		this.setTips("为 eclipse 工程自动生成 build.xml~");
		this.setDesc("<html>使用说明：<br/>" +
				"我做了限制，一次只能拽入一个 java 工程文件夹<br/>" +
				"而且对拽入的文件夹会执行检查操作，以提高操作安全性<br/>" +
				"检查：是否存在 .project, .classpath 文件，<br/>" +
				"是否存在 src 文件夹</html>");
		this.setImgName("w400_h300.png");	// 窗口大小由图片大小所决定~
	}

	@Override
	protected void handle(File f) {}
	
	@Override
	protected void handleEntries(List<File> listEntries) {
		File firstItem = listEntries.get(0);
		
		// 一次只能处理一个工程文件，而且仅在拽入的是单个文件夹的时候才有效~
		if (!firstItem.isDirectory()) {
			System.err.println("不是一个工程文件夹（目录）！");
			return;
		}
		if (listEntries.size() > 1) {
			System.err.println("一次仅处理一个工程文件夹！");
			return;
		}
		
		boolean bSrcDir = false;
		boolean bDotProject = false;
		boolean bDotClasspath = false;
		
		File[] files = firstItem.listFiles();
		for (File file : files) {
			if (file.isDirectory()) {
				if (file.getName().equals("src")) {
					bSrcDir = true;
				}
			} else {
				if (file.getName().equals(".project")) {
					bDotProject = true;
				}
				if (file.getName().equals(".classpath")) {
					bDotClasspath = true;
				}
			}
		}
		
		// 为确保操作的安全执行，必要的检查是不可忽略的~
		if (!bSrcDir || !bDotProject || !bDotClasspath) {
			System.err.println("生成 build.xml 失败，该文件夹不具有 .project、.classpath 或 src 目录~");
			return;
		}
		System.out.println("This is a valid java project directory!");
		
		// 于根目录生成 build.properties 文件~
		String propPath = PathUtil.getFullPath("ant_template.properties");
		InputStream is = this.getClass().getResourceAsStream(propPath);
		String strProp = IOBridge.stream2String(is, "utf-8");
		IOUtils.closeQuietly(is);
		strProp = strProp.replace("__APPLICATION_NAME__", firstItem.getName());
		
		File buildProp = new File(firstItem.getAbsolutePath() + File.separator + "build.properties");
		IOBridge.string2File(strProp, buildProp);
		
		// 于根目录生成 build.xml 文件~
		StringBuilder sb = new StringBuilder();
		File fLibs = new File(firstItem.getAbsolutePath() + File.separator + "libs");
		if (fLibs.exists() && fLibs.isDirectory()) {
			File[] jarFiles = fLibs.listFiles();
			for (File jarFile : jarFiles) {
				if (jarFile.getName().endsWith(".jar")) {
					sb.append("\t\t\t<zipfileset excludes=\"META-INF/*.SF\" src=\"./libs/");
					sb.append(jarFile.getName());
					sb.append("\" />\n");
				}
			}
		}
		
		String xmlPath = PathUtil.getFullPath("ant_template.xml");
		is = GenBuildXml.class.getResourceAsStream(xmlPath);
		String strXml = IOBridge.stream2String(is, "utf-8");
		IOUtils.closeQuietly(is);
		
		if (sb.toString().equals("")) {
			strXml = strXml.replace("__EXTERNAL_JARS__", "");
		} else {
			strXml = strXml.replace("__EXTERNAL_JARS__", sb.toString());					
		}
		
		strXml = strXml.replace("__PROJECT_NAME__", firstItem.getName());
		
		File buildXml = new File(firstItem.getAbsolutePath() + File.separator + "build.xml");
		IOBridge.string2File(strXml, buildXml);
	}
	
	public static void main(String args[]) {
		new GUI(new GenBuildXml());
	}
}
{% endcodeblock %}
{% codeblock ant_template.properties lang:js %}
# App name(e.g. "JavaDynamicComiple")
name=__APPLICATION_NAME__
# App version(e.g. "1.0")
version=1.0
# App author(e.g. "yang3wei")
author=yang3wei
# Desktop absolute path(e.g. "/Users/y3w/Desktop")
desktop=/Users/y3w/Desktop
# Resource package(e.g. "org/bruce/pocket/tools/res")
res.pkg=
# Classpath of entry class(e.g. "org.bruce.test.Main")
entry.class=
{% endcodeblock %}
{% codeblock ant_template.xml lang:xml %}
<project name="__PROJECT_NAME__" default="execute.class">

	<echo message="1.define variables~" />
	<!-- 
	也可以从 xml 文件中读取属性：<xmlproperty file="config.xml" />
	详细请参见：http://blog.csdn.net/jzy23682891/article/details/7063489 
	-->
	<property file="build.properties" />
	<property name="src" value="./src" />
	<property name="bin" value="./bin" />
	<property name="libs" value="./libs" />
	<property name="build" value="./build" />
	<property name="dist" value="./dist" />

	<echo message="2.define external.jars.path" />
	<path id="external.jars.path">
		<fileset dir="${libs}">
			<include name="**/*.jar" />
		</fileset>
	</path>


	<target name="prepare">
		<echo message="3.prepare" />
		<mkdir dir="${build}" />
		<mkdir dir="${dist}" />
	</target>


	<target name="compile" depends="prepare">
		<echo message="5.compile" />
		<description>将src目录下的资源文件复制到 build目录下面(保留包结构)</description>
		<delete dir="${build}/${res.pkg}" />
		<copy todir="${build}/${res.pkg}">
			<fileset dir="${src}/${res.pkg}" />
		</copy>

		<javac srcdir="${src}" destdir="${build}" encoding="UTF-8" deprecation="true" listfiles="off" fork="true" target="1.6" debug="false" failonerror="false">
			<!--给编译器指定编码，防止出现："警告： 编码 GBK 的不可映射字符"-->
			<compilerarg line="-encoding UTF-8 " />
			<classpath refid="external.jars.path" />
		</javac>
		<echo message="compile finished!" />
	</target>


	<target name="execute.class" depends="compile">
		<echo message="${name}.execute" />
		<java classname="${entry.class}" classpath="${build}" fork="true">
			<sysproperty key="file.encoding" value="UTF-8" />
			<classpath refid="external.jars.path" />
		</java>
	</target>

	<!-- 优点：所依赖的外部 jar 包的名称不用再一一写出了 -->
	<target name="package2jar" depends="compile">
		<echo message="${name}.package2jar" />
		<jar destfile="${dist}/${name}${version}.jar">
			<manifest>
				<attribute name="App-Name" value="${name}" />
				<attribute name="App-Version" value="${version}" />
				<attribute name="App-Author" value="${author}" />
				<attribute name="Created-By" value="${author}" />
				<attribute name="Main-Class" value="${entry.class}" />
			</manifest>
			<fileset dir="${build}" />
			<zipgroupfileset dir="./libs" includes="**/*.jar" />
		</jar>
	</target>

	<target name="execute.jar" depends="package2jar">
		<echo message="${name}.jar.execute" />
		<java fork="true" failonerror="true" jar="${dist}/${name}${version}.jar">
			<sysproperty key="file.encoding" value="UTF-8" />
		</java>
	</target>

	<target name="copy2desktop" depends="package2jar">
		<echo message="${name}.copy2desktop" />
		<copy file="${dist}/${name}${version}.jar" tofile="${desktop}/${name}${version}.jar" />
	</target>

	<target name="clean">
		<delete dir="${build}" />
		<delete file="${dist}/${name}${version}.jar" />
		<delete dir="${dist}" />
	</target>

	<target name="rerun" depends="clean">
		<ant antfile="build.xml" target="execute.class" />
	</target>

</project>
{% endcodeblock %}


