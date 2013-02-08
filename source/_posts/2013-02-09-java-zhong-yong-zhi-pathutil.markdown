---
layout: post
title: "Java 重用之 PathUtil"
date: 2013-02-09 01:42
comments: true
categories: Java相关
tags: [java]
---
直接上代码：  
<!-- more -->
{% codeblock PathUtil.java lang:java %}
package org.bruce.pocket.tools.utils;

import java.io.File;
import java.io.IOException;
import java.io.UnsupportedEncodingException;
import java.net.URL;
import java.net.URLDecoder;
import java.security.ProtectionDomain;
import java.util.Enumeration;
import java.util.LinkedList;
import java.util.List;
import java.util.jar.JarEntry;
import java.util.jar.JarFile;

/**
 * @author Bruce Yang
 * 路径工具~
 * 
 * 1.public static String getRunningPath() {}
 * <b>功能描述</b>：
 * 获取当前正处于执行状态的 java 程序的绝对路径:
 * 如果是以 jar 的方式执行，则返回该 jar 文件的绝对路径；
 * 	/Users/y3w/Desktop/ChangeIcon1.0.jar
 * 如果是以 eclipse 或 ant 的方式在目录中执行的，则返回 bin 或 build 目录的绝对路径~
 * 	/Users/y3w/Projects/Eclipse/workspace1/ChangeIcon/bin/
 * 	/Users/y3w/Projects/Eclipse/workspace1/ChangeIcon/build/
 * 
 * 2.public static String getRunningParentPath() {}
 * <b>功能描述</b>：
 * 获取当前正处于执行状态的 java 程序的父路径（末尾不带 File.seperator）。
 * 
 * 3.public static String getFullPath(String resFileName) {}
 * <b>功能描述</b>：
 * 在使用 jar 包内资源文件的时候，仅需传入文件名即可获得完整包路径~
 * 如传入 “ant_template.properties”，就能得到：
 * “/org/bruce/pocket/tools/res/ant_template.properties”
 * 优点就是，无论是在 eclipse 中执行，还是在 ant 中执行，甚至是在 jar 包中执行，
 * 都能表现一致，无障碍地拿到所需要资源文件的数据~
 * <b>注意事项</b>：
 * 须将资源文件放置在以 DOMAIN 开头，以 “res” 结尾的包里面~
 * <b>注</b>：
 * 借鉴了 cocos2d 的资源文件加载机制~
 * 
 * 4.
 */
public class PathUtil {
	
	/**
	 * 在这里设定以哪个包为起始点执行搜索资源文件的操作（避免在导入的其他 jar 包里面做无用功）~
	 */
	private static final String DOMAIN = "org/bruce/";
	
	private static String _runningPath;
	private static String _runningParentPath;
	
	private static final List<String> _pathList;
	static {
		_pathList = new LinkedList<String>();
		
		List<String> tempList = null;
		String absPath = getRunningPath();
		
		// /Users/y3w/Desktop/ChangeIcon1.0.jar
		if (absPath.endsWith(".jar")) {
			JarFile jarFile = null;
			try {
				jarFile = new JarFile(absPath);
			} catch (IOException e) {
				e.printStackTrace();
			}
			// [org/bruce/, org/, org/bruce/pocket/, ...]
			tempList = getFilteredFolderPaths(jarFile, DOMAIN);
			
			for (String item : tempList) {
				if (item.endsWith("/res/")) {
					_pathList.add("/" + item);
				}
			}
			
		} else {
			// [~/Projects/eclipse/workspace2/JavaDynamicCompile/org/bruce/pocket/tools/res, ...]
			// [C:\java\Projects\JavaDynamicCompile\org\bruce\pocket\tools\res, ...]
			tempList = new LinkedList<String>();
			listFolderPaths(new File(absPath), tempList);	// 绝对路径~

			// windows
			String domain = null;
			boolean bIsWindows = System.getProperty("os.name").equals("windows");
			if (bIsWindows) {
				domain = DOMAIN.replace("/", "\\\\");
			} else {
				domain = DOMAIN;
			}
			
			for (String item : tempList) {
				if (item.contains(domain) && item.endsWith(File.separator + "res")) {
					item = item.replace(absPath, "");
					if (bIsWindows) {
						_pathList.add("/" + item.replace("\\\\", "/") + "/");
					} else {
						_pathList.add("/" + item + "/");
					}
				}
			}
		}
//		System.out.println(pathList);
	}
	
	/**
	 * 由资源文件的文件命获取完整路径~
	 * 须将资源文件放置在以 DOMAIN 开头，以 “res” 结尾的包里面，不然就会找不到~
	 * @param resFileName
	 * @return
	 */
	public static String getFullPath(String resFileName) {
		String target = resFileName + "_does_Not_Exist";
		String fullPath;
		if (_runningPath.endsWith(".jar")) {
			JarFile jarFile = null;
			try {
				jarFile = new JarFile(_runningPath);
			} catch (IOException e) {
				e.printStackTrace();
			}
			for (String item : _pathList) {
				fullPath = item + resFileName;
				
				/**
				 * getJarEntry 的合法参数格式：
				 * org/bruce/pocket/tools/res/w400_h300.png
				 * 须要 target 的第一个斜杠给去掉，纠结了老子半天~
				 */
				if (jarFile.getJarEntry(fullPath.substring(1)) != null) {
					target = fullPath;
					break;
				}
			}
		} else {
			for (String item : _pathList) {
				StringBuffer sb = new StringBuffer();
				sb.append(_runningPath);
				sb.append(item.substring(1));
				sb.append(resFileName);
				if (new File(sb.toString().replaceAll("/", File.separator)).exists()) {
					target = item + resFileName;
					break;
				}
			}
		}
		return target;
	}
	/**
	 * 将 JarFile 中以特定前缀起始的 JarEntry.getName() 存放到列表中返回~
	 * @param jarFile
	 * @param strFilter
	 * @return
	 */
	private static List<String> getFilteredFolderPaths(JarFile jarFile, String strFilter) {
		List<String> target = new LinkedList<String>();
		Enumeration<JarEntry> jarEntries = jarFile.entries();
		while (jarEntries.hasMoreElements()) {
			JarEntry jarEntry = jarEntries.nextElement();
			if (jarEntry.isDirectory() && jarEntry.getName().startsWith(strFilter)) {
				target.add(jarEntry.getName());
			}
		}
		return target;
	}
	/**
	 * 将目录中所有的子目录的绝对路径存放到列表中返回~
	 * @param dir
	 * @param pathList
	 */
	private static void listFolderPaths(File dir, List<String> pathList) {
		for (File item : dir.listFiles()) {
			if (item.isDirectory()) {
				pathList.add(item.getAbsolutePath());
				listFolderPaths(item, pathList);
			}
		}
	}
	
	/**
	 * 获取当前正处于执行状态的 java 程序的绝对路径
	 * @return
	 */
	public static String getRunningPath() {
		if (_runningPath == null) {
			String strTemp = null;
			
			ProtectionDomain pDomain = PathUtil.class.getProtectionDomain();
			URL url = pDomain.getCodeSource().getLocation();
			
			try {
				strTemp = URLDecoder.decode(url.getPath(), "utf-8");
			} catch (UnsupportedEncodingException e) {
				e.printStackTrace();
			}
			
			/**
			 * Windows 下运行时会出现在首部多加一个斜杠，正反斜杠的问题~
			 * C:\>java -jar C:\ChangeFolderIcon\ChangeIcon1.0.jar
			 * PathUtil.getRunningPath() = /C:/ChangeFolderIcon/ChangeIcon1.0.jar
			 * Installer.createContainers() failed! exit!
			 */
			if (System.getProperty("os.name").startsWith("Windows")) {
				// 剔除首部的斜杠~
				strTemp = strTemp.substring(1);
				// 将斜杠替换为反斜杠~
				strTemp = strTemp.replaceAll("/", "\\\\");
			}
			_runningPath = strTemp;
//			System.out.println("PathUtil.getRunningPath() = " + runningPath);
		}
		return _runningPath;
	}
	
	/**
	 * 获取父级目录的绝对路径~
	 * @return
	 */
	public static String getRunningParentPath() {
		if (_runningParentPath == null) {
			String runningPath = PathUtil.getRunningPath();
			
			if (!runningPath.endsWith(".jar") && runningPath.endsWith(File.separator)) {
				// 移除末尾的 “/”
				int index = runningPath.lastIndexOf(File.separator);
				runningPath = runningPath.substring(0, index);
			}
			
			// 移除最后一个路径元部件~
			int index = runningPath.lastIndexOf(File.separator);
			_runningParentPath = runningPath.substring(0, index);
//			System.out.println("PathUtil.getRunningParentPath() = " + runningParentPath);
		}
		return _runningParentPath;
	}
	
	/**
	 * @param args
	 * 输出为：
	 * /Users/y3w/Projects/Eclipse/workspace2/JavaDynamicCompile/bin/
	 * /Users/y3w/Projects/Eclipse/workspace2/JavaDynamicCompile
	 * /org/bruce/pocket/tools/res/w400_h300.png
	 * /org/bruce/pocket/tools/res/BruceYang.jv
	 */
	public static void main(String args[]) {
		System.out.println(getRunningPath());
		System.out.println(getRunningParentPath());
		System.out.println(getFullPath("w400_h300.png"));
		System.out.println(getFullPath("BruceYang.jv"));
	}
}
{% endcodeblock %}
