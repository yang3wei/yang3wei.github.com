---
layout: post
title: "Java Get classes In Package"
date: 2013-02-05 12:30
comments: true
categories: Java相关
tags: [java]
---
直接上代码：  
<!-- more -->
{% codeblock GetClassesInPackage.java lang:java %}
package org.bruce.pocket.tools.utils;

import java.io.FileInputStream;
import java.util.ArrayList;
import java.util.List;
import java.util.jar.JarEntry;
import java.util.jar.JarInputStream;

/**
 * @author Bruce Yang
 * http://www.roseindia.net/java/java-get-example/get-classes-package.shtml
 */
public class GetClassesInPackage {
	private static boolean getJar = true;

	public static List<String> getClasseNamesInPackage(String jarName, String packageName) {
		if (getJar) {
			System.out.println("Jar \"" + jarName + "\" for \"" + packageName + '\"');
		}
		
		ArrayList<String> arrayList = new ArrayList<String>();
		// replaceAll 第一个参数是正则字符串，第二个参数是普通的替换字符串，不需要对正则进行转义~
		packageName = packageName.replaceAll("\\.", "/");
		
		try {
			JarInputStream jarFile = new JarInputStream(new FileInputStream(jarName));
			JarEntry jarEntry;
			
			while (true) {
				jarEntry = jarFile.getNextJarEntry();
				if (jarEntry == null) {
					break;
				}
				if ((jarEntry.getName().startsWith(packageName)) 
						&& (jarEntry.getName().endsWith(".class"))) {
					arrayList.add(jarEntry.getName().replaceAll("/", "."));
				}
			}
		} catch (Exception e) {
			e.printStackTrace();
		}
		return arrayList;
	}

	public static void main(String[] args) {
		String jarPath = "/Users/user/Desktop/JavaDynamicComiple.jar";
		String pkgPath = "org.bruce.pocket.tools.items";
		List<String> list = GetClassesInPackage.getClasseNamesInPackage(jarPath, pkgPath);
		System.out.println("Found: ");
		for (String item : list) {
			System.out.println(item);
		}
	}
}
{% endcodeblock %}
