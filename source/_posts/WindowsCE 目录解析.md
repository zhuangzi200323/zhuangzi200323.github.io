---
title: WindowsCE 目录解析
date: 2008-03-20 15:06:15
img: '/medias/5.jpg'
categories:
- WinCE
---

原文地址 \[www.cnblogs.com\](https://www.cnblogs.com/nasiry/archive/2004/09/22/45471.html)

## WindowsCE 目录解析  
 ---windowsCE.net 4.20 篇  
                 ---------------by nasiry    

windowsCE 的目录结构巨大而庞杂，尽管 windowsCE 的帮助对这个庞大的目录结构进行了说明仍然有很多目录下对应的代码在 windowsCE help 没有介绍其具体用途，下面就将 windowsCE 的目录结构进行简单的介绍。  
1.SDK  
在 windowsCE.net4.2 的根目录下存在 PLATFORM,PRIVATE,SDK,OTHERS,PUBLIC 五个目录。SDK 是其中最为简单的一个目录结构 SDK\\BIN\\I386 下所存放的就是开发所需要的工具集，编译器等等，在这里我就不再啰嗦了。  
The SDK directory contains tools that are used by Platform Builder. However,  
it does not contain the tools that are used to build the image. The tools  
contained by the SDK directory are:  
.. Processor compilers  
This directory contains the cross compilers and assemblers for supported  
platforms.  
.. Development tools  
This directory contains the tools such as, Error Lookup, GuidGen, and Link  
and Nmake.  
.. Miscellaneous utilites  
This directory contains the utilities, such as Zoomin, and Windiff.

2.other  
这个目录下的内容比较杂，有诸如 ATL，MFC 的头文件，运行库等开发应用程序所需的库文件和其一部分代码。其中有个文件目录对开发系统来说是比较重要的，就是 CETK 的客户端 OTHERS\\WCETK，通过该目录下的文件可以订制自己的 Windows CE .NET Test Kit 客户端。  
The Others directory contains all the Windows CE runtime libraries and  
headers, components for .NET Compact Frameworks, and Samples for ATL  
and MFC. The Others directory contains the following subdirectories:  
.. ATL  
This directory contains ATL headers, libraries and source code for  
debugging. The library debug symbols are also included.  
.. DOTNET  
This directory contains the .NET processor specific binaries and portable  
.NET Compact Framework files.  
.. MFC  
This directory contains MFC Headers, Libraries and Source Code for  
debugging. The library debug symbols are also included.  
.. SAMPLES  
This directory contains the MFC and ATL sample applications.  
.. WCETK  
This directory contains the Windows CE Test Kit (CETK) processor  
specific binaries. CETK provides tests for each class of devices. Within the  
CETK, you can choose the appropriate test to perform on your device driver  
and the tool remotely runs the tests on your target device.

3.PLATFORM  
PLATFORM 下存放的就是传说中的 BSP 了，这些 BSP 提供对各种开发板的硬件抽象层，bootloader,soc 驱动等资源，由此可见这个目录下的资源是硬件相关的，一般说来该目录下每个文件夹都对应一个 SDB 的，下面以 SMDK2410 为例说明 platform 目录结构的内容.  
|platform|--smdk2410----|--CESYSGEN  
   |--DRIVERS------|--DISPLAY  
   |  |--DRVLIB   
   |  |--KEYBD  
   |  |--PCMCIA  
   |  |--PWRBTN  
   |  |--SERIAL  
   |  |--TOUCHP  
   |  |--USB  
   |  |--WAVCLICK  
   |  |--WAVEDEV  
   |--EBOOT--------|--ARM  
   |  |--BIN    
   |--FILES--------|--INTLTRNS  
   |--GWE----------|--BUILDEXE  
   |  |--NLEDDRV  
   |--INC  |  
   |--KERNEL-------|--BUILDEXE-----|--KERN  
   |  |  |--KERNKITL  
   |  |  |--KERNKITLPROF  
   |  |--HAL  
   |  |--PROFILER  
   |--NAND\*  
   |--UTILITIES\*  
其中 NAND 和 UTILITIES 是 2410 的 bsp 特有的，NAND 包含的是以 NANDFLASH 为启动介质的启动代码，UTILITIES 里面的内容是包含调试信息的可执行文件 - 三星的测试程序 (用 ADS 的调试器 AXD 就可以使用该文件)。  
CESYSGEN 一般都只有一个叫 makefile 的文件，通常该文件指向 $(\_WINCEROOT)\\public\\common\\cesysgen\\CeSysgenPlatform.mak 也就是公共的 platform 生成文件，该文件为产生 windowsCE 项目提供支持。  
DRIVER 目录下的内容也是很直接的，也就是各种 soc 的设备和板载设备的驱动。例如：display 就是 lcd 控制器的驱动，keybd 就是键盘驱动等等  
eboot 目录下面的文件是 ethernet bootloader 的源文件，通常这个目录下的文件都来自于 PUBLIC\\COMMON\\OAK\\DRIVERS\\ETHDBG 所提供的模版改写得到，因此发现该目录下文件使用到未知的函数时都可以到上述文件中寻找答案。(bootloader 的源文件是完整的，有兴趣的男男女女都大可研究一下)  
FILES 下面就是一个杂物仓库，里面可能有 BSP 提供的应用程序和生成系统所需要的信息文件。要是想要知道该 bsp 的入口点，调整镜像，了解静态高内存分配等信息大可以到这儿一看，INTLTRNS 子目录下定义了一些字串，这些字串在生成系统的时候用来替换 CEPB 生成注册表所使用的标示。  
GWE 里面就是 GWES 的部分了，这里面 NLEDDRV 下面的文件是 LED 提示灯的驱动，至于 buildExe 就是生成 gwes.exe 所需的文件了，事实上这个目录下是空的，因为使用 CSP 里面的 GWS.lib 等所提供的接口已经完备了。  
INC 就是头文件的聚集地，这个就不说了。  
KERNEL:  
   这个目录结构是整个 bsp 的关键所在，内核就在这儿诞生。  
   buildexe 目录下的三个文件夹 - KERN-KERNKITL--KERNKITLPROF 分别生成 kernel 的三个版本 kernel only/kernel with kitl transport/kernel with kitl and profiler，第一个版本就是最基本的 kernel 也就是以后用作发布用的，第二个提供了 kitl 传输界面，可对系统进行调试, 而 kernkitlprof 提供了传输界面的同时还提供了专门的性能计时器用作测定系统性能。  
   PROFILER 目录下面的就是性能计时器的代码。  
   HAL 下面是内核硬件抽象层的代码所在。  
   基本上 platform 文件夹就是这个样子了，比起后面还没有说的 private 和 public 目录来说这两个文件夹的结构还是相对简单明了的。  
   我们看看后面都还有什么东西

4.PUBLIC  
这个文件夹是我这辈子见到过的最最可怕的目录结构之一。总而言之这里面什么东西都有，一个字 "杂"，难说得上什么分类。先来看看第一级子目录的结构吧。  
|public-|--WCESHELLFE \*  
 |--SHELLSDK   \*   
 |--SHELL      \*   
 |--WCEAPPSFE   
 |--VIEWERS  
 |--SQLCE  
 |--SPEECH  
 |--SERVERS  
 |--SCRPIT  
 |--IE  
 |--RDP  
 |--NETCF  
 |--IABASE  
 |--HLBASE  
 |--DirectX  
 |--DCOM  
 |--DATASYNC  
 |--COMMON  
COMMON 下面的东西很多，现在我们先绕开不说。我还是从上往下说吧 (流汗中...)  
4.1 关于 shell  
WCESHELLFE，SHELLSDK，SHELL 这三个目录下的东西都是 shell 相关的 (别问我什么是 shell, 它就是你天天看到，最最熟悉你还不知道它的存在的那个玩意儿).SHELL 下面提供了 standshell 和 AYGSHELL，standShell 就是标准的 windowsCEshell 看起来和 tablePC 的样子差不多，其中 AYGshell 原来是给 Pocket PC 设计的, 这两个 shell 都可以在 CEPB 的 IDE 里面选。  
至于 WCESHELLFE，主要是各种 windowsShell 的组件：  
CTLPNL 就是控制面板的组件，有了这个你可以自己做自己的控制项，且不愉快？  
FTPUPDATE 提供了通过 ftp 升级 firemire 的程序，作用也是给大家照猫画虎  
LANG 和 INPUTMETHODS 是输入法支持，这个我不了解就不乱说了  
Taskman 这个就比较有意思，恐怕很多人都会需要这个东西，用这个 shell 可以直接生成一个空的桌面，没有图标，没有 icon, 除了蓝色的桌面什么都没有，不过这个不能通过 IDE 选项生成，照帮助也是弄不出来的，需要修改 sysgen.bat 才行。  
至于这个目录下面的那些个什么 FILE ,inc,lib 与上面提到的同名的文件夹作用差不多，我就不再说了.  
shellSDK 就比较简单了，里面也就是开发 SHELL 应用程序所要用的 head 和 lib，没啥好讲了。  
Modules and components relative to the far-east enabled  
shell, such as, task manager and control panel components

  
4.2 WCEAPPSFE  
传说这里面有 word 和 outlook，我没弄过，也不清楚。  
既然别人这么说我就当它有吧。

4.3 VIEWERS  
这个听名字就知道是干什么的，也就是 windowsCE 带的那一对查看器，比如什么 pdf 啊，excel 啊, word 啊什么的，不过都不是 source 都只是 lib 了，所以也没什么大的意思的。

4.4 SQLCE  
这个就是 SQLCE 的老家，不过和上面一样，还是没得改，更没得写，一样的无聊。

4.5 SPEECH  
这个目录下面是 MS SAPI5.0, 里面包括了 TTS 和语音识别，免费的啊！不过只支持汉语和英语还有日语的 TTS 其他的语言一概不支持。同样也是不开放代码，顺便说一句，file 里面有个 voice 目录，那个是发声文件，要想听别的声音就要自己换，要是谁知道哪儿有这些 vce 文件请告诉我一下。

4.6 SERVERS  
这个目录下主要是 windowsCE 的网络服务。上次记得有人问我类似路有器的远程管理界面 (web shell) 是怎么弄的，在这儿找答案吧。

4.7 SCRPIT  IE  
这个不用说了就是天天用的 Internet Explorer 还有 java-vb script 支持所在。其中 IE 是可以订制的。

4.8 RDP  
该目录下面提供远程桌面的支持 (我理解为用于 windowsCE 的木马)，包括远程软件的客户端和服务端。里面有个第三方软件，具体的使用方法我也不是很清楚。

4.9 NETCF  
Dot Net Compact Framework. 事实上目录下面是空的，到底这些. net 的文件都在那儿呢？看看 makefile 就可以知道，这里面用到的资源都是来自于前面讲过的 %WINCEROOT%\\OTHERS\\DOTNET 下面。  
4.10 IABASE  
4.11 HLBASE  
这两个合到一起讲，这两个目录存在的目的不是给 PB 的 IDE 用的，而是在命令行模式下创建工程的模版，里面有很多. bat 文件，对应了一些默认的配置，这个和 PB IDE 里面的向导是一致的。两个目录的区别在于，一个带显示一个不带，这就决定了配置有很多相关的东西不一样：如 gwes 等，所以分开两个目录，使用不同的 sysgen.bat 文件。在 CE5.0 中已经将这两个目录合二为一了。  
Internet Appliance Base Configuration  
Headless Base Configuration

4.12 DirectX  
DirectX 就是 DirectX 了，看看目录结构  
├─CESYSGEN  
Sysgen 目录下  
├─OAK  
│  ├─FILES  
│  │  ├─INTLFILE  
│  │  └─INTLTRNS  
│  ├─INC  
│  ├─LIB  
│  └─VOIP  
│      ├─VOIPMEDIA\_REF  
│      ├─VOIPPROV  
│      │  ├─FILE  
│      │  ├─LDAP  
│      │  ├─MANUAL  
│      │  ├─MANUAL\_CORE  
│      │  ├─TEST  
│      │  └─XMLHELPER  
│      └─VOIPUI  
│          ├─APP  
│          │  └─RES  
│          ├─HELPER  
│          ├─IME  
│          ├─INC  
│          └─STATE  
└─SDK  
    ├─INC  
    ├─LIB  
    └─SAMPLES  
        ├─D3D  
        │  ├─BOIDS  
        │  ├─SIMPLE  
        │  └─TUTORIALS  
        │      ├─TUT01\_CREATEDEVICE  
        │      ├─TUT02\_VERTICES  
        │      ├─TUT03\_MATRICES  
        │      ├─TUT04\_LIGHTS  
        │      └─TUT05\_TEXTURES  
        ├─DDRAW  
        │  ├─INCLUDE  
        │  └─SRC  
        │      ├─DDEX1  
        │      ├─DDEX2  
        │      ├─DDEX3  
        │      ├─DDEX4  
        │      ├─DDUTIL  
        │      ├─DONUTS  
        │      └─MOSQUITO  
        ├─DDVD  
        │  ├─ANXJTEST  
        │  ├─DVDPLAY  
        │  └─DVDSAMPLE  
        ├─DSHOW  
        │  ├─BASECLASSES  
        │  │  ├─HEADED  
        │  │  └─HEADLESS  
        │  ├─FILTERS  
        │  │  ├─BALL  
        │  │  ├─DUMP  
        │  │  ├─EZRGB24  
        │  │  └─INFTEE  
        │  └─PLAYERS  
        │      ├─CEPLAYIT  
        │      ├─DDXCLMV  
        │      ├─HLPLAYIT  
        │      └─PLAYWND  
        ├─DSOUND  
        │  ├─CAPTUREECHO  
        │  └─DSPLAY  
        ├─DXMISC  
        │  ├─DDCAPS  
        │  └─DXVIEW  
        ├─MIXER  
        │  └─TMIX  
        ├─STB  
        │  └─STBIHOST  
        ├─VOIP  
        │  └─VOIPDEMO  
        │      ├─RESOURCES  
        │      └─SOURCE  
        └─WMP  
            └─CEPLAYER  
                └─RES

4.13 DCOM  
该目录提供 DCOM/COM 的支持，可惜的是没有源码，仅仅是 2 二进制的链接库。值得注意的是 SDK 目录下面的 sample 目录下提供比如：regsvr32 等等常用的工具。

4.14 DATASYNC  
Device-side communication components.

5 public\\common\\OAK\\driver  
不要以为这个目录下的东西就像它的名字一样全部都是驱动，事实上这个目录下面有很多驱动和 windows 组件，我们在 windowsCE IDE 中选取的很多特性的源代码都在这里，而且这儿提供了很多开发模版供我们写代码使用，个人认为用好这个目录是我们是定制自己的 windowCE 的关键，特色 windowsCE 就从这里开始。:)  
到这里顺便说一个事情，OAK 这个目录名一直困扰我多年，直到前段时间有位叫做 "土豹子" 的兄弟告知才明白，oak 原来是 OEM adaptation kit 的意思。  
下面我们就来看看这下面都有什么东西吧。  
├─1394  
│  ├─DRIVERS  
│  │  ├─CLIENT  
│  │  │  ├─1394DCAM  
│  │  │  ├─AVC\_STREAM  
│  │  │  └─AVC\_VCR  
│  │  └─VIRTUAL  
│  │      ├─1394DCAM  
│  │      ├─AVC\_UNIT  
│  │      └─AVC\_VVCR  
│  ├─INC  
│  └─TEST  
│      ├─DRIVER  
│      │  ├─1394DIAG  
│      │  ├─1394VDEV  
│      │  └─COMMON  
│      ├─INC  
│      └─UI  
│          └─TOPOMAP  
1394 驱动。  
├─ACM  
│  ├─ACMDWRAP  
│  ├─GSM610  
│  ├─MSFILTER  
│  └─PCMCONV  
├─BATTDRVR  
│  └─TEST  
│      ├─GETBAT  
│      └─SETSTUBBAT  
ACM 是 Audio Compression Manager 的简写也就是音频压缩管理器，该目录下面所提供了 G.711 (A-law 和 mu-law 编码), GSM 610, G.722.1, 和 SIREN，说白了就是音频编码器, 做传输用途做 voip 这类的东西时还是比较有用的。GSM610 在同名目录下，另外的几个 ACM 在 MSFILTER 目录下。PCMCONV 故名思义就是 PCM converter 了.  
├─BATTDRVR  
电池驱动。  
├─BLOCK  
│  ├─ATADISK  
│  ├─ATAPI  
│  ├─MSFLASHFMD  
│  │  ├─ECC  
│  │  ├─INC  
│  │  ├─SDNPCI  
│  │  │  ├─SDNPCID  
│  │  │  └─SDNPCIK  
│  │  └─STRATA  
│  │      ├─STRATAD  
│  │      └─STRATAK  
│  ├─RAMDISK  
│  │  ├─DRIVER  
│  │  └─LOADERDriver Code Structure  
│  └─SDMMC  
BLOCK 目录下面就是 windowsCE 提供的所有的 block 驱动。包括 PCCARD 接口的硬盘 / 光驱，Flash，以及 ramdisk 和 SD card /MMC card 的驱动程序。  
ATAPI 是 PCI 总线的 ATA 控制器驱动，尽管只有一个目录内容却是不少的这里面包含了 ALI 的 1543，promise 的 20262，intel 82371ATA 控制器 / 南桥芯片的驱动程序。ATADISK 是 PCMCIA ATA 卡的驱动。另外还有 PCI 总线界面的 SD/NANDFLASH/MMC 卡，以及平面接口的 NORFLASH 的驱动程序。RAMDISK 是使用内存模拟存储系统的例子。  
├─BLUETOOTH  
│  ├─DRIVERS  
│  │  ├─WCESTREAMBT  
│  │  └─WENDYSER  
│  ├─PROFILES  
│  │  └─HID  
│  │      ├─BASE  
│  │      ├─PARSER  
│  │      └─PB  
│  ├─SAMPLE  
│  │  ├─ASUI  
│  │  ├─BTCP  
│  │  ├─BTDC4.15 COMMON  
│  │  ├─BTDIALER  
│  │  ├─BTDIALP  
│  │  ├─BTECHO  
│  │  ├─BTENUM  
│  │  ├─BTFILTER  
│  │  ├─BTHEADSET  
│  │  │  ├─BTAUDIOGW  
│  │  │  ├─BTHSAGW\_CE  
│  │  │  ├─BTHSAGW\_MDD  
│  │  │  ├─HEADSETCTL  
│  │  │  ├─INC  
│  │  │  ├─PLAY  
│  │  │  └─TALK  
│  │  ├─BTHLINK  
│  │  ├─BTHNSCREATE  
│  │  ├─BTLOADER  
│  │  ├─BTMODEM  
│  │  ├─BTPAIR  
│  │  ├─BTSVC  
│  │  ├─COMTEST  
│  │  ├─DUNPORT  
│  │  ├─FCTEST  
│  │  ├─GW  
│  │  ├─HCITEST  
│  │  ├─L2CAPAPI  
│  │  ├─L2CAPDEV  
│  │  ├─OPTTEST  
│  │  ├─PHONEUI  
│  │  ├─PRINTUI  
│  │  ├─SCA  
│  │  ├─SDPCOMMON  
│  │  ├─SDPPRINT  
│  │  ├─SDPSEARCH  
│  │  ├─SOCKTEST  
│  │  └─SSA  
│  └─TRANSPORTS  
│      ├─AMBICOM  
│      ├─BCSP  
│      ├─SC  
│      ├─UART  
│      ├─UNIV  
│      ├─USB  
│      └─WSS  
│          └─INC  
这个目录下的是关于蓝牙的支持，我没看过，也就不乱说了。  
├─CALIBRUI  
这个目录是一个触摸屏校准的例子。  
├─CEDDK  
│  ├─DDK\_BUS  
│  ├─DDK\_DMA  
│  ├─DDK\_IO  
│  ├─DDK\_MAP  
│  ├─DDK\_TIME  
│  ├─DLL  
│  └─TEST  
│      ├─ISAENUM  
│      └─PCIENUM  
由于嵌入式系统的硬件架构千差万别所以在定制 DDK 的时候需要为自己的硬件系统专门实现一套一般操作的硬件抽象层，以供其它开发者使用这些一般的操作能够控制挂接的设备，这些操作通常包括总线, DMA,IO, 计时器等资源，以该目录的为一套 x86 的 DDK 的接口抽象。  
├─CERDISP  
│  ├─CERDISP  
│  ├─CERHOST  
│  └─INC  
远程显示的例子。这个远程显示就是 windowsCE 特性里面的那个。  
├─DISPLAY  
│  ├─AABLT  
│  ├─ATI  
│  ├─DDGPE  
│  ├─DISPPERF  
│  ├─EMUL  
│  ├─EMULROTATEPERM3  
│  ├─EMULSTUB  
│  ├─GPE  
│  ├─GPEROTATE  
│  ├─MQ200  
│  ├─NOPDISP  
│  ├─PERM3  
│  ├─PERM3\_CFG  
│  ├─RFLAT  
│  ├─SWG  
│  ├─TVIA5000  
│  └─VGAFLAT  
显示部分的东西就比较多了，aablt 是抗锯齿的例子，DDGPE 是带 DDraw 的 GPE 驱动的例子，DISPPERF 是测试驱动效率的一段代码，夹在自己的程序里面可以做测用。，SWG 是 Microsoft Software Graphics Library 的源代码，没有看完不知道齐不齐。以 mul 开头的都是模拟图形处理源代码，还有 gpe 的源代码，带旋转能力的 gpe 的源代码。上面能说的这几个在 display 目录下的程序都不是驱动程序，应该算是 windowsCE GWES 显示部分的一些源代码尽管没有提供完整的代码但从这些个零散的代码也能看出 windowsCE 的图形库的基本结构了，还是很有研究价值的，更况且这个和 winnt 的架构是如此的相似: P  
NODISP 是个比较特殊的显示驱动的例子，它满足 GDI 的借口需求但同时并没有实际的显示，有 PERM3 啥用我也没想明白。  
rflat 下面是基于 GPERotate 的显示驱动，想要旋状你的屏幕又不想改硬件的可以来看看。至于 vgaflat 就是模拟器上使用的那个驱动程序。这段提到的这几个驱动程序都是最最简单的显示驱动程序模版，如果需要自己写显示驱动可以拷贝这些代码保留架构重新实现就可以了，看得出微软在做这些个目录结构的时候还是为大家考虑得比较周到的。这几个驱动都是用在 VESA BIOS 2.0 兼容的显卡上的就是说模拟器就可以使用这些驱动，要是想操练显示驱动又没有硬件支持的话，可以用这几个驱动作试验。

剩下的那几个是几种特定的显卡的驱动，PERM3 是个比较特别的驱动，这是这些驱动里面唯一支持 D3D 的显示驱动程序。尽管微软推荐用这几个驱动作为写代码的参考，我还是建议大家用上面提到的 XXflat 的显示驱动来重新实现会比较清晰。  
├─ETHDBG  
│  ├─3C90X  
│  ├─AM79C970  
│  ├─BLCOMMON  
│  ├─BOOTPART  
│  ├─CS8900  
│  ├─DEC21140  
│  ├─DP83815  
│  ├─EBOOT  
│  ├─EDBGSAMP  
│  ├─KITLETH  
│  ├─NE2000  
│  ├─RNE\_MDD  
│  ├─RTL8139  
│  ├─SMC9000  
│  ├─SMSC100  
│  ├─VBDGSTUB  
│  ├─VBRIDGE  
│  └─VMINI  
ETHDBG 下面有很多东西，同样是比较杂.  
blcommon，bootpart，eboot 都是写 bootloader 所需要的组成部分。我们可以看到 platform 下面的 eboot 的代码调用了很多其它的函数，那些函数就是这里的代码生成的，看不懂 bootloader？接着这儿的代码一起看相信很快就可以看出结果。blcommon 是 bootloader 的主流程代码，bootpart 是 bootloader 操作 hdd 或 Flash 的相关代码，主要是在文件系统层面进行操作比如分区啊什么的，eboot 下面包含的内容是 tftp 和 dhcp 等网络协议的实现，但不包含 hal 代码。KITLETH 是 kitl 下以太网 transport 的硬件无关实现，包含 DHCP 和 UDP 协议的实现。

EDBGSAMP 下面应该是 TCP/Ip transport 的实现，目的是通过以太网卡实现调试信息通道，过对这个我没有去验证过不是很确定。

3C90X AM79C970 CS8900 DEC21140 DP83815 NE2000 RTL8139 SMC9000 SMSC100 都是针对芯片的驱动代码，bootloader 可以利用它们作为 ethernet 和 KITL 的 HAL 但是必须将这些 "驱动" 与 windowsCE 的驱动程序区别开来，这些驱动仅仅是能用于作为操作系统下的 HAL 使用，而不是系统网络部分的网卡驱动。

RNE\_MDD KITLETH  VBDGSTUB VBRIDGE VMINI 都是关于 kitl 的代码，KITLETH 是 kitl 的以太网传输实现，vbridge 和 vmini 是虚拟网桥和网卡，这两个部分的目的在于建立 kitl ethernet transport 的同时 windowsCE 能共享以太网用作传输。vDBGSTUB 是 vbirdge 的最简实现 (啥都不干)。  
RNE\_MDD 是 R-ndis 的 MDD 支持。可以与 Common\\Oak\\Drivers\\Rndisfn\\net2890 配合使用.  
├─FSD  
│  ├─ENCFILT  
│  ├─FATUTIL  
│  │  ├─MAIN  
│  │  ├─NOUI  
│  │  └─UI  
│  ├─FSDSPY  
│  └─REGPART  
FSD 不用说，就是文件系统驱动了，这个是上层的驱动程序与具体的硬件代码无关。FATUTIL 提供了一系列的支持，比如格式化，磁盘扫描这类的程序。ENCFILT 里面提供了加密的途径，要加密这里来。而 FSDSPY 估计是侦测文件系统的，具体使用方法和用途不详。REGPART 是一个 partition 驱动的例子，作用同样也是模版。构建好自己的 partition 驱动以后可以像调用 mspart.dll 来操作。  
├─GDCFG  
这个是 Generic Device Configuration 是和 PCI 总线相关的代码，看代码大概是总线设备通常行为管理和控制的用途，由于知识的匮乏，不知道具体干什么用的。  
├─GIISR  
Generic installable ISR 的例子。  
├─HELPER  
传闻是注册已配置的 PCI 设备的代码，同样我不清楚。  
├─IMEJPN  
├─IMEJPP  
│  ├─COMMONCODE  
│  ├─DICMAKE  
│  │  └─BIN  
│  └─IMEUI  
这两个目录都是输入法 - IME 相关的，还是不懂。  
├─IRMAC  
│  ├─INC  
│  ├─IRSIR  
│  └─UTILS  
红外线端口的驱动程序。  
├─KEYBD  
│  ├─DEVICELAYOUTS  
│  │  ├─PS2\_AT  
│  │  │  ├─00000409  
│  │  │  ├─00000411  
│  │  │  ├─00000412  
│  │  │  └─00010411  
│  │  └─REMAP  
│  │      └─NUMPADRMP  
│  ├─DLL  
│  │  ├─KBD8042JPN1  
│  │  ├─KBD8042JPN2  
│  │  ├─KBD8042KOR  
│  │  ├─KBD8042US  
│  │  ├─KBDNOPJPN1  
│  │  ├─KBDNOPJPN2  
│  │  ├─KBDNOPKOR  
│  │  └─KBDNOPUS  
│  ├─HIDIOCTL  
│  ├─INPUTLANGS  
│  │  ├─0409  
│  │  ├─0411  
│  │  └─0412  
│  ├─IST  
│  ├─LAYMGR  
│  ├─NOP  
│  │  ├─PDD  
│  │  └─PDDLIST  
│  ├─PS2\_8042  
│  └─TEST  
键盘驱动及其测试程序。  
├─LMEMDEBUG  
这个目录下的代码可以使用用户代码进行内存检测，通常用于测试内存泄露和调试，有兴趣还可以参考 private\\winceos\\coreos\\core\\lmem 目录下的内容。  
├─MESSAGEDIALOGBOXCUSTOMIZE  
这个下面就是大名鼎鼎的 MessageBox, 想要自己的 messageBox？容易啊，改掉这个家伙就可以了。  
├─MMTIMER  
多媒体时钟。  
├─NDISUIO  
NDIS usermode I/O driver. 用途不明。  
├─NETCARD  
│  ├─DC21X4  
│  ├─DP83815  
│  ├─NE2000  
│  ├─NE2000ISR  
│  ├─NETMUI  
│  ├─RNDISMINI  
│  │  ├─HOST  
│  ├─SMSC100FD  
│  └─XIRCOM.CE2  
这个目录下面的是网卡驱动. NE2000ISR 相当于一个插件使得 ne2000 本地驱动可以工作在 IISR 模式下。netmui 据说是网络多用户支持里面的内容简单得看不出怎么个支持法。RNDISMINI 是 RNDIS 的小端口驱动，这相当于一个实现了微软的 remote NDIS Specification 的 MDD，这个程序与 RNDISFN 目录下的 PCI-USB 界面结合就可以实现 rNDIS，RNDISFN 目录下面的东西相当于是 PDD，Host 目录下面的东西是使用 RNDISFN 作为 PDD 时，在 PC 上所需的安装信息文件，另外所需的 RNDIS-USB Kit 在微软的网站上可以下到。  
├─NETSAMP  
│  ├─ASYNCMAC  
│  ├─AUTORAS  
│  │  ├─DLL  
│  │  └─TEST  
│  ├─CONNMC  
│  ├─ETHMAN  
│  ├─INC  
│  ├─IPCONFIG  
│  ├─IPNAT  
│  │  └─EDITSAMP  
│  ├─IPV6TUN  
│  ├─NDISCONFIG  
│  ├─NDISPWR  
│  ├─NETSTAT  
│  ├─PASSTHRU  
│  ├─PEGTERM  
│  │  ├─APP  
│  │  └─TERMCTRL  
│  ├─PING  
│  ├─RASDIAL  
│  ├─RASENTRY  
│  ├─RASSERVER  
│  ├─RNAAPP  
│  ├─ROUTE  
│  ├─TAPIHELP  
│  ├─TRACERT  
│  ├─WZCTOOL  
│  └─ZCLOG  
这个目录下面的东西包括了整个广域网协议从上到下的各个层面的一些例子和工具，ping netstat route tracert ipconfig 这几个是大家再熟悉不过的网络工具了。其他的我也不是很清楚用途。  
├─NETUI  
这个目录下面的是 UI 相关于网络的部分，比如控制面板里面的网络选项，在状态栏出现的连接图标等等。  
├─OOMUI  
OOMUI 就是 out of memory User Interface... 内存溢出时显示什么东西？就在这儿了。顺便说一句所有以 UI 结束的目录都是可定制 UI 的部分。  
├─PARALLEL  
这里有并行口模块驱动的代码，以及使用该模块驱动生成并口驱动的例子。  
├─PCIBUS  
这个目录下是 PCI 总线相关的代码，不了解我就不说了。  
├─PCMCIA  
│  ├─I82365  
│  ├─MDD  
│  ├─TEST  
│  │  ├─DEVTUPLE  
│  │  └─TUPLETST  
│  ├─TI1250  
│  └─TI1250ISR  
同样，MDD 下面就是 PCMCIA 的模块驱动，另外 I82365 和 ti1250 是使用模块驱动构建驱动程序的例子, TI1250ISR 是 TI1250 驱动的 IISR.  
├─PM  
│  ├─BATTERY  
│  ├─MDD  
│  ├─PDD  
│  │  ├─DEFAULT  
│  │  └─PDA  
│  ├─PMSTUBS  
│  └─TEST  
│      ├─DEVSAMPLE  
│      ├─GET  
│      ├─GETD  
│      ├─MON  
│      ├─REQ  
│      ├─SET  
│      └─SETD  
PM 就是 power management，里面包含电池驱动. PM device 的模块驱动，test 下面还有使用模块驱动的例子。PDD 目录下是平台相关的驱动代码。  
├─PRINTER  
│  ├─PCL  
│  ├─PRNERR  
│  └─PRNPORT  
看名字像是打印机相关的驱动, 具体内容不详.  
├─REGENUM  
设备管理器的注册表枚举器, 该目录下的内容可以生成一个 regenum.dll，该链接库由设备管理器加载并决定了如何使用注册表的信息来加载驱动，因此在很多场合 (由硬件决定的不同的加载方式) 都需要自己订制该链接库，这样才能正确地加载驱动。  
├─RNDISFN  
│  └─NET2890  
使用 2890 建立 r-NDIS 的例子。  
├─SAMPTSPI  
TAPI 的实现样例。  
├─SERIAL  
│  ├─2890\_SER  
│  ├─COM16550  
│  ├─COM\_CARD  
│  ├─COM\_MDD  
│  ├─COM\_MDD2  
│  ├─ISR16550  
│  ├─SER16550  
│  ├─SER\_CARD  
│  └─SER\_CARD2  
这个目录下都是串行驱动的例子，2890\_SER 是使用 USB 端口模仿普通串口的行为，这样子就可以通过 RAS 用 USB 端口与桌面 PC 机相连接，这是最流行的用法，很多 ARM 系列的处理器 BSP 都对 USB function 实现成 Serial-unimoden 的形式，桌面的驱动可以在 activeSync 目录下找到，仅仅需要修改驱动的安装信息文件就可以直接使用，所以这个驱动是针对 ActiveSync 的实现，如果需要将 USB 实现为 RNDIS 的话所参考的驱动就不再是这个目录下的代码而是 RNDIS 下的相应代码了，再进一步如果需要将该设备用于你自定义的功能的话 <比如与你自己的 USB 桌面程序同步 / 下载> 就需要直接将该设备实现为流式驱动，由此可以看到 WindowsCE 的驱动的实现并不时一尘不变的，实现的方法由目的决定，是多样的。com16550 是标准的 CE 串行驱动范例，COM\_CARD 是 PCMICA 界面 COM 口的例子。COM\_MDD/2 是串口模块驱动，通常我们并不直接位操作系统实现流式驱动供其调用，而是编写与 MDD 接口相兼容的代码然后再让 MDD 区实现流式驱动。这样来我们仅仅需要将特定的硬件操作封装成其所需要的程序就可以了而不需要做与系统相关的诸多工作，该目录下的代码就是串行口模块驱动的源代码。ISR16550 是 IISR 的例子。ser16550 是把 16550 当作标准设备来使用的驱动的例子，这个驱动是不使用模块驱动的。SER\_CARD 下的驱动是使用 ser16550 生成的库编写的 PCMICA 界面的串行口的驱动。这个目录下面并没有列举多个芯片的驱动，而是以 16550 为例子对其进行了多种方式的实现，对学习 windowsCE 驱动程序结构还是很有研究价值的。  
├─SKINNABLEUI  
│  ├─COMMCTRL  
│  ├─COMMCTRLXP  
│  ├─GWE  
│  │  ├─BTNCTL  
│  │  ├─CMBCTL  
│  │  ├─CURSOR  
│  │  ├─GCACHE  
│  │  ├─INC  
│  │  ├─LBCTLVIEW  
│  │  ├─NCLIENT  
│  │  ├─SBCMN  
│  │  └─STCCTL  
│  └─GWEXP  
│      ├─BTNCTLVIEWXP  
│      ├─CMBCTLVIEWXP  
│      ├─GCACHEVIEWXP  
│      ├─INC  
│      ├─LBCTLVIEWXP  
│      ├─NCLIENTVIEWXP  
│      ├─SBCMNVIEWXP  
│      └─STCCTLVIEWXP  
这个目录下面是定值 UI 界面的例子，不带 XP 后缀的是 WinCE 下 win9X 风格的公共控件和光标等的代码，而带 XP 后缀则是 winXP UI 元素的代码。诸如：光标，button，static,checkbox 等  
├─SMARTCARD  
│  ├─BULLTLP3  
│  ├─PSCR  
│  ├─SMCLIB  
│  └─STCUSB  
SmartCArd 相关的驱动。  
├─SNMP  
│  └─SNMPTESTMIB  
简单网络管理协议的测试软件代码，据说仅仅是对 MIB 的测试。  
├─STARTUI  
可以通过改写编译这下面的代码改变启动时运行的程序的外观和功能。  
├─TCH\_CAL  
这个目录下的代码是用于为 2D 触摸屏驱动提供坐标旋转校准的能力。  
├─TEST  
│  └─PCMCIA

├─TOUCH  
│  ├─BASIC  
│  ├─TCHMAIN  
│  └─TRNSCRBR  
这里面包括了触摸屏的 MDD 和 DDI 等代码。  
├─USB  
│  ├─CLASS  
│  │  ├─COMMON  
│  │  ├─HID  
│  │  │  ├─CLIENTS  
│  │  │  │  ├─CONSHID  
│  │  │  │  ├─KBDHID  
│  │  │  │  └─MOUHID  
│  │  │  └─HIDCLASS  
│  │  ├─PRINTER  
│  │  └─STORAGE  
│  │      ├─CLASS  
│  │      ├─DISK  
│  │      │  └─SCSI2  
│  │      ├─emulatorDbg  
│  │      └─INC  
│  ├─CLIENTS  
│  ├─COMMON  
│  ├─HCD  
│  │  ├─COMMON  
│  │  ├─OHC  
│  │  ├─OHCD2  
│  │  ├─UHC  
│  │  └─UHCD  
│  ├─INC  
│  └─USBD  
该目录下面是 USB 相关的驱动，class 下面的都是类驱动，包括人机界面 (也就是键盘鼠标), 打印机，磁盘 / 存储设备的类驱动。HCD 下面的代码是 OHCD 和 UHCD 的驱动代码。USBD 下面是 USB 驱动代码。  
├─VGACON  
这个目录下很有用，如果你不需要 GUI 而是需要控制台的时候，就可以使用这个目录下的驱动.  
├─WAVEDEV  
│  ├─MDD  
│  ├─PDD  
│  │  ├─ES1371  
│  │  └─TVIA5000  
│  └─UNIFIED  
│      └─ENSONIQ  
声卡驱动，包含 MDD 和 ES1371 TVIA5000 的 MDD, 至于 Unified 下面是 Unified Audio Model 驱动的例子.  
└─WAVEUI  
和前面的 Calibrui, Fatui, Netui, Oomui, Startui 等一样，waveui 也是用户自定义界面的一部分，不过这次是相关声音输出的。

  
to be continue....  

Prviate 目录非完全收录  
WINCE420\\PRIVATE\\WINCEOS\\COREOS\\NK\\TOOLS\\ROMIMAGE  
├─CATBIN  
├─CHECKSYMBOLS  
├─COMPBIN  
├─COMPRESS  
├─COMPRESSLZ77  
├─COMPRESSLZX  
├─CVRTBIN  
├─DATA  
├─DIFFBIN  
├─MYSTRING  
├─ROMIMAGE  
├─SIGNBIN  
│  ├─CE  
│  └─NT  
├─SORTBIN  
├─STAMPBIN  
└─VIEWBIN  
这个目录下面收录的都是对 windowsCE 镜像进行操作的工作，但这些工具不是在 windowsCE 上运行的，而是桌面 PC 的程序。