---
title: 电源SUSPEND 过程
date: 2008-05-30 15:06:15
img: '/medias/12.jpg'
categories:
- WinCE
---

## 电源SUSPEND 过程

一般情况下，电源SUSPEND 过程是

1、Key SUSPEND EVENT 发生，系统陷入到服务线程中，调用系统SET(...SUSPEND)

2、系统陷入到 POWER.C 中调用OEMPowerOff() 做一些寄存器的保存工作

3、调用CPUPowerOff 函数，此函数一般在 \*.s 文件中做最后SUSPEND 前的工作，包括保存相关现场，设置SDRAM自刷新、设置唤醒中断源等，最后发送CPU挂起命令，系统进入最后的挂起状态。

4、等待外部唤醒中断发生，系统自动进入POWERWAKEup 处理代码，一般情况下 最早也是\*.S文件中，最后，系统进入OemPowerOff 函数，紧跟者上次最后一条指令继续执行，恢复相关寄存器等。给各个驱动程序发送消息，重新加载各个需要加载的驱动程序。