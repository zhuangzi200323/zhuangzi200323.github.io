---
title: 查看 Ubuntu 的版本号，可以采用以下方法
date: 2010-05-10 15:06:15
img: '/medias/2.jpg'
categories:
- Ubuntu
tags: 
- Ubuntu
---

方法一：
在终端中执行下列指令：
``` bash
$ cat /etc/issue
```

可以查看当前正在运行的 Ubuntu 的版本号。其输出结果类似下面的内容：
``` bash
Ubuntu 10.04 LTS \n \l
```

方法二：
使用 lsb_release 命令也可以查看 Ubuntu
的版本号，与方法一相比，内容更为详细。执行指令如下：
``` bash
sudo lsb_release -a
```

将输出结果：
``` bash
No LSB modules are available.
Distributor ID: Ubuntu
Description: Ubuntu 10.04 LTS
Release: 
10.04
Codename: 
Lucid
```