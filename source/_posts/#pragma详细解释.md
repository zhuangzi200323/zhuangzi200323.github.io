---
title: '#pragma详细解释'
date: 2008-03-18 15:06:15
img: '/medias/15.jpg'
categories:
- MFC
---


## #pragma详细解释

#Pragma是预处理指令，它的作用是设定编译器的状态或者是指示编译器完成一些特定的动作。#Pragma指令对每个编译器给出了一个方法,在保持与C和C++语言完全兼容的情况下,给出主机或操作系统专有的特征。依据定义,编译指示是机器或操作系统专有的,且对于每个编译器都是不同的。

其格式一般为: #Pragma Para
其中Para 为参数，下面来看一些常用的参数。

(1)message 参数。 Message 参数是我最喜欢的一个参数，它能够在编译信息输出窗口中输出相应的信息，这对于源代码信息的控制是非常重要的。其使用方法为：
#Pragma message(“消息文本”)
当编译器遇到这条指令时就在编译输出窗口中将消息文本打印出来。当我们在程序中定义了许多宏来控制源代码版本的时候，我们自己有可能都会忘记有没有正确的设置这些宏，此时我们可以用这条指令在编译的时候就进行检查。假设我们希望判断自己有没有在源代码的什么地方定义了_X86这个宏可以用下面的方法
```CPP
#ifdef _X86
#Pragma message(“_X86 macro activated!”)
#endif
```
当我们定义了_X86这个宏以后，应用程序在编译时就会在编译输出窗口里显示“_
X86 macro activated!”。我们就不会因为不记得自己定义的一些特定的宏而抓耳挠腮了。

(2)另一个使用得比较多的pragma参数是code_seg。格式如：
```CPP
#pragma code_seg( [\section-name\[,\section-class\] ] )
```
它能够设置程序中函数代码存放的代码段，使用没有section-name字符串的#pragmacode_seg可在编译开始时将其复位，当我们开发驱动程序的时候就会使用到它。

(3)#pragma once (比较常用）
只要在头文件的最开始加入这条指令就能够保证头文件被编译一次，这条指令实际上在VC6中就已经有了，但是考虑到兼容性并没有太多的使用它。

(4)#pragma hdrstop表示预编译头文件到此为止，后面的头文件不进行预编译。BCB可以预编译头文件以加快链接的速度，但如果所有头文件都进行预编译又可能占太多磁盘空间，所以使用这个选项排除一些头文件。有时单元之间有依赖关系，比如单元A依赖单元B，所以单元B要先于单元A编译。你可以用#pragma startup指定编译优先级，如果使用了#pragma package(smart_init) ，BCB就会根据优先级的大小先后编译。

(5)#pragma resource \*.dfm\表示把*.dfm文件中的资源加入工程。*.dfm中包括窗体外观的定义。

(6)#pragma warning( disable : 4507 34; once : 4385; error : 164 )
等价于：
#pragma warning(disable:4507 34) // 不显示4507和34号警告信息
#pragma warning(once:4385) // 4385号警告信息仅报告一次
#pragma warning(error:164) // 把164号警告信息作为一个错误。
同时这个pragma warning 也支持如下格式：
#pragma warning( push [ ,n ] )
#pragma warning( pop )
这里n代表一个警告等级(1---4)。
#pragma warning( push )保存所有警告信息的现有的警告状态。
#pragma warning( push, n)保存所有警告信息的现有的警告状态，并且把全局警告
等级设定为n。
#pragma warning( pop )向栈中弹出最后一个警告信息，在入栈和出栈之间所作的
一切改动取消。例如：
#pragma warning( push )
#pragma warning( disable : 4705 )
#pragma warning( disable : 4706 )
#pragma warning( disable : 4707 )
//.......
#pragma warning( pop )
在这段代码的最后，重新保存所有的警告信息(包括4705，4706和4707)。

（7）pragma comment(...)
该指令将一个注释记录放入一个对象文件或可执行文件中。
常用的lib关键字，可以帮我们连入一个库文件。

（8）·通过#pragma pack(n)改变C编译器的字节对齐方式
在C语言中，结构是一种复合数据类型，其构成元素既可以是基本数据类型（如int、long、float等）的变量，也可以是一些复合数据类型（如数组、结构、联合等）的数据单元。在结构中，编译器为结构的每个成员按其自然对界（alignment）条件分配空间。各个成员按照它们被声明的顺序在内存中顺序存储，第一个成员的地址和整个结构的地址相同。
例如，下面的结构各成员空间分配情况：
struct test
{
        char x1; // 偏移地址为0
        short x2;// 偏移地址为[2，3]
        float x3;// 偏移地址为[4，7]
        char x4; // 偏移地址为8
};
      结构的第一个成员x1，其偏移地址为0，占据了第1个字节。第二个成员x2为short类型，其起始地址必须2字节对界，因此，编译器在x2和x1之间填充了一个空字节。结构的第三个成员x3和第四个成员x4恰好落在其自然对界地址上，在它们前面不需要额外的填充字节。在test结构中，成员x3要求4字节对界，是该结构所有成员中要求的最大对界单元，因而test结构的自然对界条件为4字节，编译器在成员x4后面填充了3个空字节。整个结构所占据空间为12字节。更改C编译器的
缺省字节对齐方式在缺省情况下，C编译器为每一个变量或是数据单元按其自然对界条件分配
空间。一般地，可以通过下面的方法来改变缺省的对界条件：
　　· 使用伪指令#pragma pack (n)，C编译器将按照n个字节对齐。
     · 使用伪指令#pragma pack ()，取消自定义字节对齐方式。
        另外，还有如下的一种方式：
     · __attribute((aligned (n)))，让所作用的结构成员对齐在n字节自然边界上。如果结构中有成员的长度大于n，则按照最大成员的长度来对齐。
     · __attribute__ ((packed))，取消结构在编译过程中的优化对齐，按照实际占用字节数进行对齐。
以上的n = 1, 2, 4, 8, 16... 第一种方式较为常见。
应用实例
　　在网络协议编程中，经常会处理不同协议的数据报文。一种方法是通过指针偏移的
方法来得到各种信息，但这样做不仅编程复杂，而且一旦协议有变化，程序修改起来
也比较麻烦。在了解了编译器对结构空间的分配原则之后，我们完全可以利用这
一特性定义自己的协议结构，通过访问结构的成员来获取各种信息。这样做，
不仅简化了编程，而且即使协议发生变化，我们也只需修改协议结构的定义即可，
其它程序无需修改，省时省力。下面以TCP协议首部为例，说明如何定义协议结构。
其协议结构定义如下：
#pragma pack(1) // 按照1字节方式进行对齐
struct TCPHEADER
{
        short SrcPort; // 16位源端口号
        short DstPort; // 16位目的端口号
        int SerialNo; // 32位序列号
        int AckNo; // 32位确认号
        unsigned char HaderLen : 4; // 4位首部长度
        unsigned char Reserved1 : 4; // 保留6位中的4位
        unsigned char Reserved2 : 2; // 保留6位中的2位
        unsigned char URG : 1;
        unsigned char ACK : 1;
        unsigned char PSH : 1;
        unsigned char RST : 1;
        unsigned char SYN : 1;
        unsigned char FIN : 1;
        short WindowSize; // 16位窗口大小
        short TcpChkSum; // 16位TCP检验和
        short UrgentPointer; // 16位紧急指针
};
#pragma pack() // 取消1字节对齐方式

#pragma pack规定的对齐长度，实际使用的规则是：
结构，联合，或者类的数据成员，第一个放在偏移为0的地方，以后每个数据成员的对齐，按照#pragma pack指定的数值和这个数据成员自身长度中，比较小的那个进行。
也就是说，当#pragma pack的值等于或超过所有数据成员长度的时候，这个值的大小将不产生任何效果。
而结构整体的对齐，则按照结构体中最大的数据成员 和 #pragma pack指定值 之间，较小的那个进行。

指定连接要使用的库
比如我们连接的时候用到了 WSock32.lib，你当然可以不辞辛苦地把它加入到你的工程中。但是我觉得更方便的方法是使用 #pragma 指示符，指定要连接的库:
#pragma comment(lib, "WSock32.lib")