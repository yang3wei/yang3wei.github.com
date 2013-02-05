---
layout: post
title: "Java 图片缩放工具类"
date: 2013-02-05 12:48
comments: true
categories: Java相关 
tags: [java]
---
直接上代码：  
<!-- more -->
{% codeblock ImageResizer.java lang:java %}
package org.bruce.pocket.tools.utils;

import java.awt.Image;
import java.awt.image.BufferedImage;
import java.awt.image.RenderedImage;
import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

import javax.imageio.ImageIO;

import com.sun.image.codec.jpeg.JPEGCodec;
import com.sun.image.codec.jpeg.JPEGImageEncoder;

/**
 * @author Bruce Yang
 * 调整图片大小~
 */
public class ImageResizer {

	public static void main(String args[]) {
		ImageResizer.resizeJpgImage("c:/test.JPG", "c:/test4.jpg", 400, 300);
	}
	
	/**
	 * 图像缩放 png 格式~
	 * @param srcImgPath 原图片文件路径
	 * @param destImgPath 生成的缩略图片文件路径
	 * @param destWidth 生成图片的宽度
	 * @param destHeight 生成图片的高度
	 */
	public static void resizePngImage(String srcImgPath, String destImgPath, int destWidth, int destHeight) {
		resizePngImage(new File(srcImgPath), new File(destImgPath), destWidth, destHeight);
	}
	public static void resizePngImage(File srcFile, File destFile, int destWidth, int destHeight) {
		try {
			if (!srcFile.exists()) {
				return;
			}
			
			// .DS_Store 这个文件害我调了半天，主要是 这个方法里面不包含文件名判断的逻辑，直接就给报错中断执行了~
//			System.err.println(srcFile.getAbsolutePath());
			
			BufferedImage srcBi = ImageIO.read(srcFile);
			// Image.SCALE_SMOOTH 的缩略算法 生成缩略图片的平滑度的 优先级比速度高 生成的图片质量比较好 但速度慢~
			Image img = srcBi.getScaledInstance(destWidth, destHeight, Image.SCALE_SMOOTH);
			
			BufferedImage targetBi = new BufferedImage(destWidth, destHeight, BufferedImage.TYPE_INT_RGB);
			targetBi.getGraphics().drawImage(img, 0, 0, null);
			
			writePNGImage(targetBi, destFile.getAbsolutePath());
		} catch (IOException ex) {
			ex.printStackTrace();
		}
	}
	

	/**
	 * 图像缩放 jpg 格式~
	 * @param srcImgPath 原图片文件路径
	 * @param destImgPath 生成的缩略图片文件路径
	 * @param destWidth 生成图片的宽度
	 * @param destHeight 生成图片的高度
	 */
	public static void resizeJpgImage(String srcImgPath, String destImgPath, int destWidth, int destHeight) {
		resizeJpgImage(new File(srcImgPath), new File(destImgPath), destWidth, destHeight);
	}
	public static void resizeJpgImage(File srcFile, File destFile, int destWidth, int destHeight) {
		try {
			if (!srcFile.exists()) {
				return;
			}
			Image srcImg = ImageIO.read(srcFile);
			BufferedImage targetBi = new BufferedImage(destWidth, destHeight, BufferedImage.TYPE_INT_RGB);
			
			// Image.SCALE_SMOOTH 的缩略算法 生成缩略图片的平滑度的 优先级比速度高 生成的图片质量比较好 但速度慢~
			Image img = srcImg.getScaledInstance(destWidth, destHeight, Image.SCALE_SMOOTH);
			targetBi.getGraphics().drawImage(img, 0, 0, null);
			
			FileOutputStream fos = new FileOutputStream(destFile);
			JPEGImageEncoder encoder = JPEGCodec.createJPEGEncoder(fos);
			encoder.encode(targetBi);
			fos.close();
		} catch (IOException ex) {
			ex.printStackTrace();
		}
	}

	/**
	 * 由高清背景图片生成 ipad 所用背景图片~
	 * 将 2560 * 3840 的图片截除上下两部分，缩放为原来的 0.3 倍（得到的图片的大小为 768 * 1024）~
	 * @param imgsrc
	 * @param imgdist
	 */
	public static void genIpadBg(String srcImgPath, String destImgPath) {
		File srcImgFile = new File(srcImgPath);
		File destImgFile = new File(destImgPath);
		genIpadBg(srcImgFile, destImgFile);
	}
	public static void genIpadBg(File srcImgFile, File destImgFile) {
		try {
			if (!srcImgFile.exists()) {
				return;
			}
			BufferedImage srcBi = ImageIO.read(srcImgFile);
			srcBi = srcBi.getSubimage(0, 213, srcBi.getWidth(), 3414);
			Image img = srcBi.getScaledInstance(768, 1024, Image.SCALE_SMOOTH);
			
			BufferedImage destBi = new BufferedImage(768, 1024, BufferedImage.TYPE_4BYTE_ABGR);
			destBi.getGraphics().drawImage(img, 0, 0, null);
			
			ImageIO.write(destBi, "png", destImgFile);
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	/**
	 * 根据图片路径 读取图片文件
	 * @param fileName
	 * @return
	 */
	public static BufferedImage readImage(String fileName) {
		try {
			BufferedImage bi = null;
			bi = ImageIO.read(new File(fileName));
			return bi;
		} catch (IOException e) {
			e.printStackTrace();
			return null;
		}
	}
	
	/**
	 * 生成新的图片文件
	 * @param im
	 * @param formatName
	 * @param fileName
	 * @return
	 */
	public static boolean writeImage(RenderedImage ri, String fileformat, String fileName) {
		boolean result = false;
		try {
			result = ImageIO.write(ri, fileformat, new File(fileName));
		} catch (IOException e) {
			e.printStackTrace();
		}
		return result;
	}

	/**
	 * 转换图片格式 到 PNG
	 * @param im
	 * @param fileName
	 * @return
	 */
	public static boolean writePNGImage(RenderedImage ri, String fileName) {
		return writeImage(ri, "PNG", fileName);
	}
	
	/**
	 * 转换图片格式 到 jpg
	 * @param im
	 * @param fileName
	 * @return
	 */
	public static boolean writeJPEGImage(RenderedImage ri, String fileName) {
		return writeImage(ri, "JPEG", fileName);
	}
	/**
	 * @param bi
	 * @param destFile
	 * @return
	 */
	public static boolean writeJPEGImage(BufferedImage bi, File destFile) {
		try {
			FileOutputStream fos = new FileOutputStream(destFile);
			JPEGImageEncoder encoder = JPEGCodec.createJPEGEncoder(fos);
			encoder.encode(bi);
			fos.close();
			return true;
		} catch (Exception e) {
			e.printStackTrace();
			return false;
		}
	}

	/**
	 * 转换图片格式 到 gif 不知到好用不
	 * @param im
	 * @param fileName
	 * @return
	 */
	public static boolean writeGIFImage(RenderedImage ri, String fileName) {
		return writeImage(ri, "GIF", fileName);
	}

	/**
	 * 转换图片格式 到 BMP
	 * @param im
	 * @param fileName
	 * @return
	 */
	public static boolean writeBMPImage(RenderedImage ri, String fileName) {
		return writeImage(ri, "BMP", fileName);
	}
}
{% endcodeblock %}
