---
title: WINCE驱动程序快速入门
date: 2008-04-18 15:06:15
img: '/medias/5.jpg'
categories:
- WinCE
---

## WINCE驱动程序快速入门


有人也许和我一样比较心急，想尽快知道如何去写一个具体的驱动程序，这里，假设您对驱动程序已经有比较好的了解，告诉大家一个快速上路的方法。当然，如果您有足够的时间我建议在动手之前还是深入了解一下CE整个系统架构。
在CE中，最简单的一个驱动程序莫过于一个内置（Built-in）设备的流接口驱动。对于一个不支持热拔插的设备，最快捷的方法就是为其实现一个内置的流接口的驱动。
对于这样一类驱动程序，我们只需要按一种特定的规则实现一个动态库，其中实现对所有的硬件功能的调用，再将这个动态库加入系统中，然后设置相关的注册表项，使得在系统启动时设备管理器能识别并且加载这个设备即可。
3．1 实现动态链接库
此动态链接库与应用程序层所用的库并不很大差别，源文件可以是C、C++、甚至汇编，，只是它要实现以下函数。
 DllEntry(HINSTANCE DllInstance, INT Reason, LPVOID Reserved )
这个函数是动态链接库的入口，每个动态链接库都需要输出这个函数，它只在动态库被加载和卸载时被调用，也就是设备管理器调用LoadLibrary而引起它被装入内存和调用UnloadLibrary将其从内存释放时被调用， 因而它是每个动态链接库最早被调用的函数，一般用它做一些全局变量的初始化。

参数：
DllInstance：DLL的句柄，与一个EXE文件的句柄功能类似，一般可以通过它在得到DLL中的一些资源，例如对话框，除此之外一般没什么用处。
Reason:
一般我们只关心两个值：DLL_PROCESS_ATTACH与DLL_PROCESS_DETACH，Reason等于前者是动态库被加载，等于后者是动态库被释放。
所以，我们可以在Reason等于前者是初始化一些资源，等于后者时将其释放。
 DWORD XXX_Init(　LPCTSTR pContext,　LPCVOID lpvBusContext);
它是驱动程序的动态库被成功装载以后第一个被调用的函数。其调用时间仅次与DllEntry,而且，当一个库用来生成多于一个的驱动程序实例时仅调用一次DllEntry，而xxx_Init会被调用多次。驱动程序应当在这个函数中初始化硬件，如果初始化成功，就分配一个自已的内存空间(通常用结构体表示)，将自已的状态保存起来，并且将此内存块的地址做为一个DWORD值返回给上层。设备管理器就会用在调用XXX_Open时将此句柄传回，我们就能访问自已的状态。如果初始化失败，则返回0以通知这个驱动程序没有成功加载，先前所分配的系统资源应该全部释放，此程序的生命即告终至。
当这个函数成功返回，设备管理器对这个程序就不做进一步处理，除非它设置了更多的特性。至此一个各为XXX的设备就已经加载成功，当用户程序调用CreateFile来打开这个设备时，设备管理器就会调XXX_Open函数。

参数：
pContext：系统传入的注册表键，通过它可以讲到我们在注册表中设置的配置信息。
lpvBusContext：一般不用，在这先不做讲解
实际上，很多程序中将这个函数写成了DWORD XXX_Init(　DWORD pContext )
我们只需要将pContext转化成LPCTSTR即可。

 DWORD XXX_Open(DWORD hDeviceContext,DWORD dwAccess, DWORD dwShareMode);
当用户程序调用CreateFile打开这个设备时，设备管理器就会调用此驱动程序的XXX_Open函数。
参数：
hDeviceContext XXX_Init 返回给上层的值，也就是我们在XXX_Init中分配的用来记录驱动程序信息的那个结构体的指针，我们可以在这个函数中直接将其转化成所定义的结构，从而获取驱动程序的信息。
dwAccess 上层所要求的访问方式，可以是读或者写，或者是0，即不读也不写
dwShareMode 上层程序所请求的共享模式，可以是共享读、共享写这两个值的逻辑或，或者是0，即独占式访问。
系统层对设备文件的存取权限及共享方法已经做了处理，所以在驱动程序中对这两个参数一般可以不用理会。
这个函数一般不用做太多处理，可以直接返回hDeviceContext表示成功，对于一个不支持多个用户的硬件，在设备已经打开后，应该总是返回0以至失败，则CreateFile调用不成功。

 DWORD XXX_Close(　 DWORD hDeviceContext　);
当用户程序调用CloseHandle关闭这个设备句柄时，这个函数就会被设备管理器调用。
参数：
hDeviceContext 为XXX_Open返回给上层的那个值。
这个函数应该做与XXX_Open相反的事情，具体包括：释放XXX_Open分配的内存，将驱动程序被打开的记数减少等。

 DWORD XXX_Deinit(　DWORD hDeviceContext　);
这个函数在设备被卸载时被调用，它应该实现与XXX_Init相反的操作，主要为释放前者占用的所有系统资源。
参数：
hDeviceContext XXX_Init函数返回给上层的那个句柄
 void XXX_PowerUp( DWORD hDeviceContext );
 void XXX_PowerDown(DWORD hDeviceContext );
正如其名称中体现的那样，这两个函数在系统PowerUp与PowerDown时被调用，这两个函数中不能使用任何可能引起线程切换的函数，否则会引起系统死机。所以，在这两个函数中，实际上几乎是什么做不了，一般在PowerDown时做一个标志，让驱动程序知道自已曾经被Power Down过。在Power Down/On的过程中硬件可能会掉电，所以，尽管Power On以后，原来的IO操作仍然会从接着执行，但可能会失败。这时，当我们发现一次IO操作失败是因为程序曾经进入过Power Down状态，就重新初始化一次硬件，再做同样的IO操作。
 BOOL XXX_IOControl(
DWORD hDeviceContext,
DWORD dwCode,
PBYTE pBufIn,
DWORD dwLenIn,
PBYTE pBufOut,
DWORD dwLenOut,
PDWORD pdwActualOut
);
几乎可以说一个驱动程序的所有功能都可以在这个函数中实现。对于一类CE自身已经支持的设备，它们已经被定义了一套IO操作定，我们只需按照各类设备已经定义的内容去实现所有的IO操作。但当我们实现一个自定义的设备时，我们就可以随心所欲定义我们自已的IO操作。
参数：
hDeviceContext XXX_Open返回给上层的那个句柄，即我们自已定义的，用来存放程序所有信息的一个结构。
dwCode IO操作码，如果是CE已经支持的设备类，就用它已经定义好码值，否则就可以自已定义。
pBufIn 传入的Buffer,每个IO操作码都会定义自已的Buffer结构
dwLenIn pBufIn以字节记的大小
pBufOut，dwLenOut分别为传出的Buffer，及其以字节记的大小
pdwActualOut 驱动程序实际在pBufOut中填入的数据以字节记的大小

其中，前两个参数是必须的，其它的任何一个都有可能是NULL或0。
所以，当给pdwActualOut赋值时应该先判断它是否为一个有效的地址

3.2注册表设置
 在注册表中添加如下项目。(一般放在Platform.reg)
[HKEY_LOCAL_MACHINE\Drivers\BuiltIn\SampleDev]
"Prefix"="XXX"
"Dll"="MyDev.Dll"
"Order"=dword:1

 在BIB文件中添加项目，将所用到的文件加入BIN文件(一般放在Platform.bib)。
MyDev.dll $(_FLATRELEASEDIR)\MyDev.dll NK SH
 注：
SampleDev为任意与其它项目不重名的字符串.
每个函数名的前缀XXX可以是任意大写的字符串，只要保证与注册表中Prefix后面的值相同就行。
3.3编译程序
现在，已经知道了需要实现哪些东西，一定想知道如何去实现它。一个最直接的方法就是在platform/BSP/drivers 下新建一个目录，然后在drivers目录中的dirs文件中加入以你刚新建的目录名。
在刚新建的目录下，新建你的C源代码文件，在其中实现上面所述的函数，及其功能。新建名称分别为sources, makefile, mydev.def的文件。其内容如下：
makefile: 只需要这样一行
!INCLUDE $(_MAKEENVROOT)\makefile.def

mydriver.def文件定义需要输出的函数，这些函数能够被其它代码用动态加载的方法调用。格式：

LIBRARY MyDev(这个字符串要和将要生成的动态库的文件名一样)

EXPORTS
XXX_Init
XXX_Deinit
XXX_Open
XXX_Close
XXX_PowerOff
XXX_Power_Down
XXX_IOControl
Sources:这个文件很重要，内容也多，最基本的一个文件该有如下内容。

TARGETNAME= MyDev（指定要生成的动态库的名称）
TARGETTYPE=DYNLINK（指定要生成的是一个动态库）
（下面两项指定需要与哪些动态库链接，一般要第一项就足够了）
TARGETLIBS=$(_COMMONSDKROOT)\lib\$(_CPUINDPATH)\coredll.lib
SOURCELIBS= $(_COMMONOAKROOT)\lib\$(_CPUINDPATH)\ceddk.lib
DEFFILE=MyDev.def (指定def文件)

DLLENTRY=DllEntry（指定动态库的入口函数）

SOURCES=（请在这写上你所有源文件的名字，它们将会被编译）


好了，现在万事俱备，只剩编译啦。

3.4用一个Project文件来编译出驱动程序库文件
如果您在用CE5.0，那用一个Project来构造一个驱动程序将是一个不错的选择。即在新建一个Project时设置其类型为DLL，其它设置根据提示即可。并且可以将注册表设置放在Project所在文件夹。
3.5基本调试方法
一般驱动程序可以用DEBUG版来调试，也可以用输出调试信息的方法。我们一般用这两个函数输出调试信息：RETAILMSG和DEBUGMS，后者只能在DEBUG版中输出，而前者在RELEASE和DEBUG版中都可以输出，而且，可以在系统运行时刻根据Debug Zone选择让DEBUGMSG输出哪些调试信息。
驱动程序的调试一般可以分为以下几步：
1．看驱动程序的DllEntry是否被调用。如果这个函数被调用，说明驱动程序的文件已经在CE的image中，而且与注册表中设置的文件名相同。
2．看Init 函数是否被调用。如果它被调用，刚说明注册表设置正确。如果它没有被调用，一般是因为注册表中的Prefix设置与Init函数前面那三个字符不相同。或者def文件中没有定义Init函数。如果这个函数能够被调用，但驱动程序还是不能正确加载，请详细检查代码。