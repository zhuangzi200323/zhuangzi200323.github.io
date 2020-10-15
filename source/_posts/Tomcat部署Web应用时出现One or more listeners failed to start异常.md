---
title: Tomcat部署Web应用时出现One or more listeners failed to start异常
date: 2019-06-02 15:06:15
img: '/medias/9.png'
categories:
- spring
tags:
- Tomcat
---

## Tomcat部署Web应用时出现One or more listeners failed to start异常

Tomcat启动成功，然后在部署应用的时候产生了如下的异常：
``` bash
08-Jul-2017 21:45:26.064 严重 [RMI TCP Connection(3)-127.0.0.1]
org.apache.catalina.core.StandardContext.startInternal One or more listeners
failed to start. Full details will be found in the appropriate container log
file

08-Jul-2017 21:45:26.066 严重 [RMI TCP Connection(3)-127.0.0.1]
org.apache.catalina.core.StandardContext.startInternal Context [] startup failed
due to previous errors
```

从错误中看到是因为有listener启动失败，查看了日志发现报了这个错误：

``` bash
org.apache.catalina.core.StandardContext.listenerStart Error configuring
application listener of class
[org.springframework.web.context.ContextLoaderListener]

java.lang.ClassNotFoundException:
org.springframework.web.context.ContextLoaderListener

at
org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1285)

at
org.apache.catalina.loader.WebappClassLoaderBase.loadClass(WebappClassLoaderBase.java:1119)

…
```

提示org.springframework.web.context.ContextLoaderListener这个类找不到。jar包已经导入到工程中了，编译器也可以识别出这个类，不应该出现这样的问题啊，很懵逼啊。

**解决方法**

功夫不负有心人啊，前人栽树后人乘凉，经过搜索终于发现了一篇能解决这个问题的文章：原文链接。

衷心的感谢一下该文章的作者。接下来是操作过程：

Ctrl+Alt+Shift+S弹出Project Structure窗口，在Output Layout标签中找到Available
Elements，鼠标右击下面的元素：

![](media/d5bacb8f1559eb0a1180564a4aad1205.jpeg)

点击Put into Output
Root，执行后，在WEB-INF会增加lib目录，里面是项目引用的jar包，点击OK。再次部署应用就能成功运行了。

![](media/36bbacce598e79cbdfa49262eafad3f1.jpeg)

原文：https://blog.csdn.net/cflys/article/details/74853402
