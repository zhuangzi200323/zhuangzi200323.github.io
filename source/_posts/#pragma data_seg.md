---
title: '#pragma data_seg'
date: 2008-03-18 15:06:15
img: '/medias/14.jpg'
categories:
- MFC
---

## \#pragma data_seg


在Win16环境中，DLL的全局数据对每个载入它的进程来说都是相同的；而在Win32环境中，情况却发生了变化，DLL函数中的代码所创建的任何对象（包括变量）都归调用它的线程或进程所有。当进程在载入DLL时操作系统自动把DLL地址映射到该进程的私有空间，也就是进程的虚拟地址空间，而且也复制该DLL的全局数据的一份拷贝到该进程空间。也就是说每个进程所拥有的相同的DLL的全局数据，它们的名称相同，但其值却并不一定是相同的，而且是互不干涉的。因此，在Win32环境下要想在多个进程中共享数据，就必须进行必要的设置。在访问同一个Dll的各进程之间共
享存储器是通过存储器映射文件技术实现的。也可以把这些需要共享的数据分离出来，放置在一个独立的数据段里，并把该段的属性设置为共享。必须给这些变量赋初值，否则编译器会把没有赋初始值的变量放在一个叫未被初始化的数据段中。
#pragma data_seg预处理指令用于设置共享数据段。例如：
```CPP
#pragma data_seg("SharedDataName")
HHOOK hHook=NULL;
#pragma data_seg()
```
在#pragma data_seg("SharedDataName")和#pragma data_seg()之间的所有变量 将被访问该Dll的所有进程看到和共享。再加上一条指令#pragma comment(linker,"/section:.SharedDataName,rws"),那么这个数据节中的数据可以在所有DLL的实例之间共享。所有对这些数据的操作都针对同一个实例的，而不是在每个进程的地址空间中都有一份。
1\. #pragma data_seg()一般用于DLL中。也就是说，在DLL中定义一个共享的，有名字的数据段。最关键的是：这个数据段中的全局变量可以被多个进程共享。否则多个进程之间无法共享DLL中的全局变量。

2\. 共享数据必须初始化，否则微软编译器会把没有初始化的数据放到.BSS段中，从而导致多个进程之间的共享行为失败。

3\. 你所谓的结果正确是一种错觉。如果你在一个DLL中这么写：

```CPP
#pragma data_seg("MyData")

int g_Value; // Note that the global is not initialized.

#pragma data_seg()
```

DLL提供两个接口函数：
```CPP
int GetValue()
{
     return g_Value;
}

void SetValue(int n)
{
     g_Value = n;
}
```
然后启动两个进程A和B，A和B都调用了这个DLL，假如A调用了SetValue(5); B接着调用int m = GetValue(); 那么m的值不一定是5，而是一个未定义的值。因为DLL中的全局数据对于每一个调用它的进程而言，是私有的，不能共享的。假如你对g_Value进行了初始化，那么g_Value就一定会被放进MyData段中。换句话说，如果A调用了SetValue(5); B接着调用int m = GetValue(); 那么m的值就一定是5！这就实现了跨进程之间的数据通信！


下面看一个实际应用，用共享数据来统计应用程序启动的次数，并作相应的处理。

在应用程序的入口处：
```CPP
//控制应用程序只能启动一次
#pragma data_seg("flag_data")
   int count=0;
#pragma data_seg()
#pragma comment(linker,"/SECTION:flag_data,RWS")
```
程序中：
```CPP
if(count>1)
{
    MessageBox("已经启动了一个应用程序"，"Warning",MB_OK);
    return FLASE;
}
count++;
```