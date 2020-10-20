---
title: Ubuntu9.10下内核编译的步骤
date: 2010-01-26 15:06:15
img: '/medias/2.jpg'
categories:
- Ubuntu
---

## Ubuntu9.10下内核编译的步骤

为了不让自己忘记，留下学习的印迹以备后用，其中有参考互联网上牛人的成果,算不上原创，只供其参考。小弟初学，有不对的地方请高手指点。

编译内核步骤如下：

### 1、下载源码

在www.kernel.org 网站上可以下载最新新源码包到/urs/scr下并解压并解压。比如下载的源码包是linux-2.6.32.5.tar.bz2，用tar –jxvf linux-2.6.32.5.tar.bz2解压。

### 2、安装编译所需要的软件和库

apt-get install build-essential kernel-package libncurses5-dev，其中build-essential包括gcc、make等工具；使用make-kpkg等命令需要kernel-package包；用make menuconfig配置内核时需要libncurses5-dev包。

### 3、进行配置

有下面4种配置方法，如下：
    a．make config在终端中一项一项进行配置；
    b．make menuconfig在终端中可以使用鼠标、键盘进行配置，需要安装libncurses5-dev；
    c．make xconfig图形界面配置，需要安装libqt3-headers；
    d．make oldconfig使用默认配置。

一般使用第二种方法，在此方法下，编译进内核是*号，编译成模块是M号，不编译是空白。

配置完成后，在源码目录下会有一个.config文件，这就是配置文件，把它备份到一个地方，主要是万一内核编译出错了，可以参照这个配置文件来查找到底是哪里出错了。

### 4、进行编译

在源码目录下使用命令make –j4进行编译。

注：（-j4是让处理器分成四个线程来编译内核，速度快，若机器很牛，推荐使用）

### 5、编译完成后依次运行命令：
sudo make bzImage；编译内核。

sudo make modules；编译模块。

sudo make modules_install；安装模块。

sudo make install；安装内核。

以上命令执行完成后,内核已经编译完,而且内核树已经建立,若做linux下驱动开发,到此步已经ok了。其他版本的linux可能到此步重启机器就可以看见自己编译好的内核了，选择启动即可。但下ubuntu要想在启动界面看见自己编译好的内核，还需执行以下命令：

sudo make-kpkg clean；清理一下。

sudo make-kpkg --initrd --append-to-version mylinux1 kernel_image kernel_headers。

其中—initrd是必须的，不然生成不了initrd文件。--append生成多重镜像，也就是在启动菜单里看到的选项。mylinux1是你自己取的镜像的名字，必须小写并以数字结尾。

这样会在源码目录的上一层目录产生两个dep包，用sudo dpkg -i 选项把这两个包都安装上就好了。重启机器，就可以从自己编译好的内核启动了。如果不行的话，进入原有的内核，用sudo dpkg -r 移掉错误的内核，重新再来。

### 6、总结
A、编译内核是非常简单的事情，特别是你编译了很多次以后。
B、编译最关键的步骤是配置。你需要花相当长的时间来理解每个选项。
C、用append-to-version产生多重镜像，防止内核出错了不可弥补。
D、不要删除你的官方老内核。千万不要！


**注：**如果是只想安装一个比原有版本更高的内核版本，可以如下做：

1、下载内核源码包并解压；

2、进入源码包目录，进行配置；

3、直接执行以下命令就可以制作出dep包：

sudo make-kpkg clean；清理一下。

sudo make-kpkg --initrd --append-to-version mylinux1 kernel_image kernel_headers。

4、用sudo dpkg –i命令安装dep包，重启机器，就可以看到新内核了。进入系统后可以用uname –a查看内核版本号与编译时间。