---
title: 解决Ubuntu 16.04 Unity与任务栏消失的问题
date: 2015-10-12 15:06:15
img: '/medias/1.jpg'
categories:
- Ubuntu
---

## 解决Ubuntu 16.04 Unity与任务栏消失的问题

方法如下：
```bash
sudoapt-get update
​sudo apt-get install remove ubuntu-desktop
​sudo apt-get install unity
​sudo shutdown -r now
```