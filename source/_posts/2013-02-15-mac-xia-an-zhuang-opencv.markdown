---
layout: post
title: "Mac 下安装 OpenCV"
date: 2013-02-15 10:32
comments: true
categories: OpenCV相关
tags: [OpenCV]
---
原文链接：[Mac下安装 OpenCV](http://stefan321.iteye.com/blog/1555880)  
本人（yang3wei）的操作系统为 Mac OS X Lion 10.7.2，安装的 OpenCV 版本为 2.4.4。<!-- more -->  
在遵照这篇文章安装 OpenCV 的时候，出现了一些意外，编译执行到 94% 的时候卡住了...  
看了一下提示信息，是在 java module 出的问题，检查一番后，发现了症结所在：  
build.xml 在执行过程中找不到 lib 目录，于是切到对应的父目录里面创建了一个 lib 目录，  
再次执行 make 命令时，继续报错，大意是找不到 junit 的相关 jar 包，  
然后，我把随便找到一个 junit.jar 丢到之前创建的那个 lib 目录中，问题解决，成功编译完毕。  
##1.下载 OpenCV
```
cd ~/<my_working _directory>
svn co http://code.opencv.org/svn/opencv/trunk
```
##2.使用 CMake 编译 OpenCV
```
cd ~/opencv
mkdir release
cd release
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
￼make
sudo make install
```
##3.测试
cpp 代码：  
{% codeblock DisplayImage.cpp lang:cpp %}
#include <cv.h>
#include <highgui.h>

using namespace cv;

int main(int argc, char** argv) {
    Mat image;
    image = imread(argv[1], 1);

    if(argc != 2 || !image.data) {
        printf("No image data \n");
        return -1;
    }

    namedWindow("Display Image", CV_WINDOW_AUTOSIZE);

    imshow("Display Image", image);

    waitKey(0);

    return 0;
}
{% endcodeblock %}
cmake 代码：  
{% codeblock CMakeLists.txt lang:java %}
project( DisplayImage )
find_package( OpenCV REQUIRED )
add_executable( DisplayImage DisplayImage )
target_link_libraries( DisplayImage ${OpenCV_LIBS} )
{% endcodeblock %}
终端运行：  
```
stefan321@Lius-MacBook$  cmake .
-- The C compiler identification is GNU 4.2.1
-- The CXX compiler identification is Clang 3.1.0
-- Checking whether C compiler has -isysroot
-- Checking whether C compiler has -isysroot - yes
-- Checking whether C compiler supports OSX deployment target flag
-- Checking whether C compiler supports OSX deployment target flag - yes
-- Check for working C compiler: /usr/bin/gcc
-- Check for working C compiler: /usr/bin/gcc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
CMake Warning (dev) in CMakeLists.txt:
  No cmake_minimum_required command is present.  A line of code such as

    cmake_minimum_required(VERSION 2.8)

  should be added at the top of the file.  The version specified may be lower
  if you wish to support older CMake versions for this project.  For more
  information run "cmake --help-policy CMP0000".
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done
-- Generating done
-- Build files have been written to: /Users/stefan321/opencv
~/opencv
stefan321@Lius-MacBook$  make
Scanning dependencies of target DisplayImage
[100%] Building CXX object CMakeFiles/DisplayImage.dir/DisplayImage.o
Linking CXX executable DisplayImage
[100%] Built target DisplayImage
~/opencv
stefan321@Lius-MacBook$  ./DisplayImage 123.jpg
```
{% img center /images/posts/f7e13c67-0ebf-387e-8ea8-93ad065d8a39.png %}
{% img center /images/posts/28ca1db6-438d-3a45-8e33-572f435709c0.png %}
