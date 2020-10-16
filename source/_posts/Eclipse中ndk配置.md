---
title: Eclipse中ndk配置
date: 2015-10-12 15:06:15
img: '/medias/1.jpg'
categories:
- Android技术相关
tags: 
- Eclipse
---

## Eclipse中ndk配置

下面是使用NDK-r7在windows下配置自动编译的builders的过程。

### 1. 先下载安装android-ndk-r10e

### 2. 打开Eclipse，导入android-ndk-r10e\samples\hello-jni

### 3. 新建并配置一个Builder：

#### (a) Project->Properties->Builders->New，新建一个Builder。

#### (b) 在弹出的【Choose configuration type】对话框，选择【Program】，点击【OK】

#### (c) 在弹出的【Edit Configuration】对话框中，配置选项卡【Main】。

在“Name“中输入新builders的名称。

在“Location”中输入nkd-build.cmd的路径。（E:\android\android-ndk-r10e\ndk-build.cmd）。

在“Working Diretcoty”中输入${workspace_loc:/HelloJni}（也可以点击“Browse Workspace”来选取HelloJni目录）。

#### (d) 【Edit Configuration】对话框中，配置选项卡【Refresh】。

勾选“Refresh resources upon completion”  
 
勾选“The entire workspace”  

勾选“Recuresively include sub-folders”。  

#### (e) 【Edit Configuration】对话框中，配置选项卡【Build options】。

勾选“After a “Clean”

勾选“During manual builds”

勾选“During auto builds”

勾选“Specify working set of relevant resources”。

点击“Specify Resources…” 勾选HelloJni工程的“jni“目录，一路确认下去就完成配置了，到这里Eclipse就能够自动调用NDK编译jin目录下的C/C++代码了。
