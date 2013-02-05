---
layout: post
title: "Ant 自动构建 build.xml"
date: 2013-02-05 13:10
comments: true
categories: Java相关
tags: [java, ant]
---
直接上文件：  
<!-- more -->
{% codeblock build.xml lang:xml %}
<project name="JavaDynamicCompile" default="execute.class" basedir=".">
	<echo message="1.define variables~" />

	<property name="name" value="JavaDynamicCompile" />
	<property name="version" value="1.0" />
	<property name="author" value="yang3wei" />

	<property name="src" value="${basedir}/src" />
	<property name="libs" value="${basedir}/libs" />
	<property name="build" value="${basedir}/build" />
	<property name="dist" value="${basedir}/dist" />

	<property name="desktop" value="/Users/user/Desktop" />
	<property name="res.pkg" value="org/bruce/pocket/tools/res" />
	<property name="entry.class" value="org.bruce.pocket.tools.entry.Main" />

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
			<fileset dir="${src}/${res.pkg}">
				<include name="**/*.jpg" />
				<include name="**/*.png" />
				<include name="**/*.gif" />
				<include name="**/*.prop" />
				<include name="**/*.properties" />
			</fileset>
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

	<target name="package2jar" depends="compile">
		<echo message="${name}.package2jar" />
		<jar destfile="${dist}/${name}${version}.jar" basedir="${build}">
			<manifest>
				<attribute name="Created-By" value="${author}" />
				<attribute name="Main-Class" value="${entry.class}" />
			</manifest>
			<zipfileset excludes="META-INF/*.SF" src="./libs/antlr-2.7.4.jar" />
			<zipfileset excludes="META-INF/*.SF" src="./libs/chardet-1.0.jar" />
			<zipfileset excludes="META-INF/*.SF" src="./libs/cpdetector_1.0.10.jar" />
			<zipfileset excludes="META-INF/*.SF" src="./libs/jdom.jar" />
			<zipfileset excludes="META-INF/*.SF" src="./libs/pdfbox-1.6.0.jar" />
			<zipfileset excludes="META-INF/*.SF" src="./libs/PDFRenderer-0.9.1.jar" />
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


