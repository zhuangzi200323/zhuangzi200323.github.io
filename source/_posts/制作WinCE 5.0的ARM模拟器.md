---
title: 制作WinCE 5.0的ARM模拟器
date: 2008-04-10 15:06:15
img: '/medias/16.jpg'
categories:
- WinCE
---


## 制作WinCE 5.0的ARM模拟器


由于调试WinCE程序的需要,而微软的VS2005集成的模拟器是PPC或SmartPhone,不能满足要求,只好自己动手制作一个.Emulator 2.0带的模拟器是x86,我需要的是ARM.

从网上能搜索到的资料不多,大多都没讲清楚.历经半个月的摸索,总算是做出来了,并且在VS2005上能成功连接和调试,非常开心,第一次写博,主要也是为自己留下点资料.如果对你有帮助,请不忘记帮顶一下.

ARM 模拟器制作总结：
首先下载Device Emulator BSP
http://www.microsoft.com/downloads/details.aspx?FamilyID=474b03a6-e87d-455d-bc16-b8cf18ef39b4&displaylang=en
安装BSP后，有
Using the CE DeviceEmulator.rtf
这个文件非常有参考价值，严格按照上面说的即可基本能达到要求。

这张贴子提到DMA的解释，很有参考意义
http://www.cnblogs.com/walzer/archive/2007/05/13/744729.html#Feedback

这张贴子有一定参考价值，但依他的做出来的模拟器运行是黑屏
http://blogs.msdn.com/barrybo/archive/2006/08/21/710908.aspx
需要注意的两点:

１、需要选择Mobile HandHeld模式，如果选择Enterprise Webpad，在模拟器中加载时报OS invalid
２、不能用emulator_500.exe来加载，这个是X86，应该用emulator.exe，版本是1.0或2.0 preview
在Settting的环境设置中，增加CE_MODULES_SERIAL=1，以便能把SerDMA的驱动编译进来
把Target WinCE和KITL这两个选项去除


另外就是Emulator_500.exe是用来加载x86模拟器的，这是我前段时间一直没成功的原因，微软的工程师barrybo帮我指出此错误。
大家最好用vs2005自带的emulator1.0或到微软站点下载emulator preview 2.0