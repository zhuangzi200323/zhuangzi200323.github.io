---
title: 关于PHYSICAL_ADDRESS定义
date: 2008-04-24 15:06:15
img: '/medias/9.jpg'
categories:
- WinCE
---

## 关于PHYSICAL_ADDRESS定义


在头文件ceddk.h中可以查到下面这个语句：

typedef LARGE_INTEGER    PHYSICAL_ADDRESS，*PPHYSICAL_ADDRESS；

因此，PHYSICAL_ADDRESS就是LARGE_INTEGER，接着查看MSDN即可知所以然。