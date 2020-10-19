---
title: Windows CE 6.0 安装顺序
date: 2008-04-09 15:06:15
img: '/medias/14.jpg'
categories:
- WinCE
---


## Windows CE 6.0 安装顺序

1、先装Visual Studio 2005, 我拿到的是Professional Edition。最好别用DEFAULT安装，把组件CUSTOM一下，不然会花很多冤枉的磁盘空间。WINCE600的Platform Builder不像WINCE500是独立的，而是作为VS2005的插件，以后建立和定制OS、编译调试全部在VS2005里完成

2、安装Visual Studio 2005 Service Pack 1, 发布的地址 http://msdn2.microsoft.com/zh-cn/vstudio/bb265237.aspx
这是必须的装的，Release Note里面提到SP1提供了Windows Embedded 6.0 platform and tools support。
不同的VS2005版本(Standard / Professional / Tem Edition) 会对应到不同的下载上，不过简单点就用这个下载 http://www.microsoft.com/downloads/details.aspx?familyid=BB4A75AB-E2D4-4C96-B39D-37BAF6B5B1DC&displaylang=en 430多兆，通吃所有版本。

3、可选，安装Visual Studio 2005 Service Pack 1 Update for Windows Vista。
发布和下载网址
http://www.microsoft.com/downloads/details.aspx?FamilyID=90e2942d-3ad1-4873-a2ee-4acc0aace5b6&displaylang=en
在SP1的基础上增加对VISTA的支持。注意一定要先装SP1，再装这个，在System Requirements里强调了Required Software: licensed copy of a Visual Studio 2005 and Visual Studio 2005 Service Pack 1. 我当时以为这个包含了上面SP1，浪费了不少时间

4、安装MSDN，从VS2005的安装页里选择。新版的MSDN界面功能看起来比较酷，但CE6的Product Documentation写得实在潦草，不少细节貌似没写清楚，所以这里建议选上CE5的产品文档留着以后作参考。

5、安装Windows Embedded CE 6.0。应用开发人员可以不装，在后文会提到可以利用CE6生成的SDK来工作；但是不装的话，恐怕无法在本地MSDN得到CE6的产品文档，另外还有PUBLIC目录下丰富的application sample code了。
几个注意点：
(1) 在下图这步才可以改路径。点选组件后，然后点下面的Browse。这里不像WINCE500安装时有专门的一步来改路径。害我狂点next/back几圈才找到。


(2) CE6没有专门的一个Emualtor(x86)选项了, 我以为是包含在x86里面, 后来发现x86部分我白装了。WINCE600和VS2005的模拟器可以直接跑ARM指令，他们管这个叫ARMulator ( 貌似LINUX上先有这个东西, MS抄人家的? )。这导致了CE5 的基于x86的Emulator无法在VS2005上跑起来，得用个很绕路并且不太好用的方法，等我用顺了再写文章出来。

(2) 注意磁盘空间，我选装了ARMV4I和X86两个，用掉6.5G，比WINCE500还要大些。

6、安装Windows Embedded CE 6.0 Platform Builder Service Pack 1，同样地，应用开发人员可以不装。
发布页 http://www.microsoft.com/downloads/details.aspx?FamilyId=BF0DC0E3-8575-4860-A8E3-290ADF242678&displaylang=en
这份只升级platform builder，并不修改WINCE600目录下的代码

--------------------------------
如果BSP开发人员不打算用模拟器的话，装到这步就行了。做应用的继续往下看

7、安装Microsoft Device Emulator 2.0。发布页
http://www.microsoft.com/downloads/details.aspx?FamilyID=dd567053-f231-4a64-a648-fea5e7061303&DisplayLang=en
注意下载时有32位版本和64位版本。安装后，emulator程序仍然在C:\Program Files\Microsoft Device Emulator\1.0下面，没有再生成一个叫2.0的目录，我总感觉被微软忽悠了。

8、安装Virtual Machine Network Driver for Microsoft Device Emulator, 发布页
http://www.microsoft.com/downloads/details.aspx?FamilyID=dc8332d6-565f-4a57-be8c-1d4718d3af65&DisplayLang=en   //该发布页已过时无法找到，我把该文件自己传了一份，请从下面地址下载
http://www.cnblogs.com/Files/walzer/Virtual%20Machine%20Network%20Driver%20for%20Microsoft%20Device%20Emulator.rar
不装这个的话，模拟器是启动不起来的，黑屏。如果通过命令行模式让DeviceEmulaotr.exe加载编译好的，可以看到串口输出窗口显示卡在初始化DMA传输的步骤上。我在这个问题上卡了近一个通宵，睡一觉起来才运气好翻到这个发布页，终于弄过去的。CE6模拟器的Download和Transport方式叫"Device Emulator (DMA)"，我一开始理解成Direct Memory Access，以为微软为了解决模拟器上单步调试缓慢的问题，让内存和模拟器进程DMA方式传输什么数据呢，类似双端口RAM的方式在内存里直接传了，和ActiveSync，TCP/IP连接就无关了。后来才发现这个DMA估计是Device Emulator Adaptor，会自动去找模拟器上有什么可用的连接方式，还是TCPIP/ActiveSync/KITL/Manual等几种老方法。VISTA上没有ActiveSync了，只好用TCP/IP连接，不装这个virtual switch那就当然连不上了。

9、VISTA用户最好安装这个: Windows Mobile Device Center 6，作为ActiveSync的替代品，我预感在以后调试多半得用到
32位版：http://www.microsoft.com/downloads/details.aspx?FamilyID=83d513ea-9df9-4920-af33-3a0e2e4e7beb&DisplayLang=en
64位版：http://www.microsoft.com/downloads/details.aspx?FamilyID=6eb8d0aa-bc6b-4864-8ffe-dc26e1d9f843&DisplayLang=en

我在XP SP2和VISTA BUSINESS上都成功安装了CE6，并且各自新建了DeviceEmulator项目（现在不叫PBWorkspace了，叫OSDesigns），编译完后用CE6 Attach Device成功启动；然后再编译生成SDK，安装到XP SP2或VISTA上后，再从VS2005里从Emulator SDK新建VC项目，编译并在Emulator上单步调试，现在Emulator就不是从Attach Device连接的了，而是从Device Emulator Manager启动的，也就是说应用开发人员可以完全不用装CE6，只要装VS2005就可以了。