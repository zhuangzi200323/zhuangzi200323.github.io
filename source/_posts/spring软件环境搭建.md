---
title: spring软件环境搭建
date: 2019-06-02 15:06:15
img: '/medias/8.png'
categories:
- spring
tags:
- IntelliJ IDEA
---

## spring软件环境搭建

**软件环境：**

IntelliJ IDEA 2018.2.5 (Ultimate Edition)

JDK 1.8

Tomcat 8.0.53

### 1、 选择File--\>New--\>Project

![](media/ed30aedfd26c819e62e6a3995eb4c3f3.jpeg)

### 2、 输入项目名称

![](media/93acddbadb79dc89d36b2e8de2cd820f.jpeg)

### 3、 点击Finish会弹出下载jar的对话框

![](media/6eb9fcffd0f330dfcb9d4c3d9ca55300.jpeg)

### 4、 修改web.xml如下

![](media/15ecb583f486d585794d968424ed3786.jpeg)

### 5、 配置xxx-servlet.xml文件

![](media/d6e13338c021f2dd9b13e02e07e97268.jpeg)

### 6、 添加helloWorld.jsp文件

![](media/9a3399f5a50c90f53b267cf663c0d134.jpeg)

### 7、 编译Controller

![](media/dafc90522d1e0ba0318b75f6fe272752.jpeg)

### 8、 部署项目

![](media/478b083d16b9a6e660b3ad06eb79b08b.jpeg)

![](media/9b291dde4ad6b9a0b8df51fe0902fa30.jpeg)

![](media/838c370bb5d4c4d43191bd2f7ef0aa3b.jpeg)

![](media/8bbc5f3b2023b66f3c932889d3f1bb88.jpeg)

![](media/b6b8908d9f8a6f069bef7fe054fcfd1d.jpeg)

### 9、 运行项目

![](media/35e91f220631993a92bad0a1a8552885.jpeg)

运行时出现如下错误

![](media/516a03873a89a02462679ea39d6d2242.jpeg)

修正相关错误

![](media/f1ccd0592806bc73b798e6e254b0fd14.jpeg)

![](media/f28690c49ee6f905a48b0f08d273c347.jpeg)

![](media/302c2f3d2f24e00e9c0500b669ce07f1.jpeg)

### 10、  再次运行项目，出现下方页面，则创建项目完成

![](media/fa786fd9a6c9d1b371f08dd3f80c1f21.jpeg)

### 11、  为了后续开发避免遇到莫名其妙的问题，请做如下设置：在web\\WEB-INF\\下增加两个文件夹，分别是classes与lib。

![](media/1bbee41e058ce8365205f9a1899515b2.png)

### 12、 将项目中的lib下所有jar拷贝到web\\WEB-INF\\lib下

### 13、 将web\\WEB-INF\\classes设置输出目录，如下图所示：

![](media/04eefa5b73a7900829b517a801e3e2c0.jpeg)
