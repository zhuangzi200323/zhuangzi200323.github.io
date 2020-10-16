---
title: 树莓派 DIY 笔记之前有介绍过用 VNC 连接到树莓派的方法
date: 2016-09-27 15:06:15
img: '/medias/3.jpg'
categories:
- 树莓派
---


树莓派 DIY 笔记之前有介绍过用 VNC 连接到树莓派的方法。在 Windows
下，当然还是自带的远程桌面更便捷。如果不想用
VNC，利用远程桌面（mstsc.exe）连接树莓派，如何实现？

只需要在 raspbian 下面安装一个服务 xdrp 即可。输入如下命令安装：
``` bash
sudo apt-get install xrdp
```

然后就 Ok 了。

![](media/be7f645be1ec59795f000fee7546ecdb.jpeg)

![](media/5939f68942d87fde89539d73af8ba56a.jpeg)

转自：https://shumeipai.nxez.com/2013/10/06/windows-remote-desktop-connection-raspberry-pi.html
