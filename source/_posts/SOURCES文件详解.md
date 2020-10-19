---
title: SOURCES文件详解
date: 2008-04-18 15:06:15
img: '/medias/3.jpg'
categories:
- WinCE
---

## SOURCES文件详解


SOURCES文件是WINCE底层开发中最重要的文件之一,主要的配置项如下:

TARGETNAME,定义模块名称.
TARGETTYPE,模块的种类,可以是DYNLINK, LIBRARY,EXE.
如果TARGETTYPE是DLL,则可以定义DLLENTRY,将Dll入口定义成别的不是DLLMain的函数,如果DLL的入口是DllMain，则不需要别的定义。
如果TARGETTYPE是EXE,则可以定义EXEENTRY,用于指定EXE的入口函数.

如果TARGETTYPE是LIBRARY,则不需要定义入口函数。


INCLUDES，如果一个模块需要使用非标准路径下的头文件时,需要定义INCLUDES,用于包含更多的头文件路径,用法如下:

INCLUDES=$(INCLUDES);\new directory\...,注意定义新的INCLUDES时,需要包含INCLUDES原来的值，否则就需要包含所有可能的目录。

TARGETLIBS,SOURCELIBS用于定义该模块需要链接哪些库文件.


TARGETLIBS，如果一个库以DLL的形式提供给调用者，就需要用TARGETLIBS，它只链接一个函数地址，系统执行时会将被链接的库加载。比如coredll.lib就是这样的库文件。即动态链接。

SOURCELIBS，将库中的函数实体链接进来。即静态链接，用到的函数会在我们的文件中形成一份拷贝。


注意，内核这个执行文件是没有TARGETLIBS的，GIISR.DLL也不能有TARGETLIBS。


WINCECOD,如果将其定义为1,则编译器会为每一个文件生成.cod文件,它是一个汇编文件，调试时查看汇编代码也是一种很好的办法。

SOURCES,定义该模块需要哪些源文件.