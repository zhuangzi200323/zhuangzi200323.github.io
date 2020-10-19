---
title: Visual Studio 2008驱动开发环境搭建
date: 2009-07-21 15:06:15
img: '/medias/15.jpg'
categories:
- Visual Studio
---

## Visual Studio 2008驱动开发环境搭建


1、Setup Visual Studio 2008


2、Setup WDK(DDK)


3、Add to VS pathes to DDK include files, libs and bins.

VS2008→Tools→Options→Projects and Solutions→VC++ Directories→[Show directories for]…

4、Create new empty "Win32 project" and add source file (i.e. Windows驱动开发技术详解第一章第一个例子Driver.cpp、Driver.h).

5、Configure project properties (All Configurations)

5.1、C\C++→General→Debug Information Format=C7 Compatible (/Z7)或Program Database (/Zi)


5.2、C\C++→Preprocessor→Preprocessor Definitions=WIN32=100;_X86_=1;DBG=1或_X86_ [add also DBG for Debug config]


5.3、C\C++→Code Generation→Enable C++ Exceptions=No


5.4、C\C++→Code Generation→Basic Runtime Checks=Default


5.5、C\C++→Code Generation→Buffer Security Check=No (/GS-)


5.6、C\C++→Advanced→Calling Convention=__stdcall (/Gz)


5.7、C\C++→Advanced→Compile As=Compile as C Code (/TC) [if you are going to use plain C]


5.8、Linker→General→Output File=$(OutDir)\$(ProjectName).sys


5.9、Linker→General→Enable Incremental Linking=Default


5.10、Linker→Input→Additional Dependencies=wdm.lib或ntoskrnl.lib hal.lib $(NOINHERIT) [add here needed libs here e.g. ntoskrnl.lib hal.lib]


5.11、Linker→Input→Ignore All Default Libraries=Yes (/NODEFAULTLIB)


5.12、Linker→Manifest File→Generate Manifest=No


5.13、Linker→System→SubSystem=Native (/SUBSYSTEM:NATIVE)


5.14、Linker→System→Driver=Driver (/DRIVER)


5.15、Linker→Advanced→Entry Point=DriverEntry


5.16、Linker→Advanced→Base Address=default或0x10000


5.17、Linker - Advanced→Randomized Base Address =default


5.18、Linker→Advanced→Data Execution Prevention (DEP) =default