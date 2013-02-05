---
layout: post
title: "Java 执行 Mac(Unix) 脚本命令"
date: 2013-02-05 12:44
comments: true
categories: Java相关
tags: [java]
---
直接上代码：  
<!-- more -->
{% codeblock UnixScript.java lang:java %}
package org.bruce.pocket.tools.utils;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.io.OutputStream;
import java.io.OutputStreamWriter;

/**
 * @author Bruce Yang
 *
 */
public class UnixScript {

	/**
	 * 执行 mac(unix) 脚本命令~
	 * @param command
	 * @return
	 */
	public static int execute(String command) {
		String[] cmd = {"/bin/bash"};
		Runtime rt = Runtime.getRuntime();
		Process proc = null;
		try {
			proc = rt.exec(cmd);
		} catch (IOException e) {
			e.printStackTrace();
		}

		// 打开流
		OutputStream os = proc.getOutputStream();
		BufferedWriter bw = new BufferedWriter(new OutputStreamWriter(os));

		try {
			bw.write(command);
			
			bw.flush();
			bw.close();
			
			/** 真奇怪，把控制台的输出打印一遍之后竟然能正常终止了~ */
			readConsole(proc);
			
			/** waitFor() 的作用在于 java 程序是否等待 Terminal 执行脚本完毕~ */
			proc.waitFor();
		} catch (Exception e) {
			e.printStackTrace();
		}
		int retCode = proc.exitValue();
//		System.err.println("unix script retCode = " + retCode);
		if (retCode != 0) {
			readConsole(proc);
			System.err.println("UnixScriptUil.execute 出错了!!");
		}
		return retCode;
	}

	/**
	 * 读取控制命令的输出结果
	 * 原文链接：http://lavasoft.blog.51cto.com/62575/15599
	 * @param cmd 命令
	 * @return 控制命令的输出结果
	 * @throws IOException
	 */
	public static String readConsole(Process process) {
		StringBuffer cmdOut = new StringBuffer();
		InputStream fis = process.getInputStream();
		BufferedReader br = new BufferedReader(new InputStreamReader(fis));
		
		String line = null;
		try {
			while ((line = br.readLine()) != null) {
				cmdOut.append(line).append(System.getProperty("line.separator"));
			}
		} catch (IOException e) {
			e.printStackTrace();
		}
//		System.out.println("执行系统命令后的控制台输出为：\n" + cmdOut.toString());
		return cmdOut.toString().trim();
	}
}
{% endcodeblock %}
