---
title: MFC/Win32 中使用 GDI+
date: 2009-09-02 15:06:15
img: '/medias/16.jpg'
categories:
- MFC
---

## MFC/Win32 中使用 GDI+

一、MFC 中使用 GDI + 的方法：

1、包含头文件与库

在 stdafx.h 中加入以下三行代码：

#include "gdiplus.h"
using namespace Gdiplus;
#pragma comment(lib, "gdiplus.lib")

2、安装 GDI+

在使用 GDI + 之前要进行安装，否则程序不会报错，但绘图不成功。安装方法如下：

GdiplusStartupInput gdiplusStartupInput;
ULONG_PTR           gdiplusToken;
GdiplusStartup(&gdiplusToken, &gdiplusStartupInput, NULL);

安装完成后就可以使用 GDI + 进行绘图了……

3、卸载 GDI+

卸载调用如下函数即可

GdiplusShutdown(gdiplusToken);

二、Win32 中使用 GDI + 的方法：

1、包含头文件与库

在 stdafx.h 中加入以下三行代码：

#include <comdef.h> 这个头文件是必须的，要不编译时会有很多错误。
#include "gdiplus.h"
using namespace Gdiplus;
#pragma comment(lib, "gdiplus.lib")

第 2、3 步与 MFC 是一样的，也列在下面，方便阅读。

2、安装 GDI+

在使用 GDI + 之前要进行安装，否则程序不会报错，但绘图不成功。安装方法如下：

GdiplusStartupInput gdiplusStartupInput;
ULONG_PTR           gdiplusToken;
GdiplusStartup(&gdiplusToken, &gdiplusStartupInput, NULL);

安装完成后就可以使用 GDI + 进行绘图了……

3、卸载 GDI+

卸载调用如下函数即可

GdiplusShutdown(gdiplusToken);