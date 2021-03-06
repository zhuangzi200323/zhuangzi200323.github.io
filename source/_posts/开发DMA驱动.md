---
title: 开发DMA驱动
date: 2008-04-18 15:06:15
img: '/medias/2.jpg'
categories:
- WinCE
---

## 开发DMA驱动

使用DMA的好处就是它不需要CPU的干预而直接服务外设，这样CPU就可以去处理别的事务，从而提高系统的效率，对于慢速设备，如UART，其作用只是降低CPU的使用率，但对于高速设备，如硬盘，它不只是降低CPU的使用率，而且能大大提高硬件设备的吞吐量。因为对于这种设备，CPU直接供应数据的速度太低。

因CPU只能一个总线周期最多存取一次总线，而且对于ARM，它不能把内存中A地址的值直接搬到B地址。它只能先把A地址的值搬到一个寄存器，然后再从这个寄存器搬到B地址。也就是说，对于ARM，要花费两个总线周期才能将A地址的值送到B地址。而DMA就不同了，一般系统中的DMA都有突发（Burst）传输的能力，在这种模式下，DMA能一次传输几个甚至几十个字节的数据，所以使用DMA能使设备的吞吐能力大为增强。

使用DMA时我们必须要注意如下事实：

1.使用物理地址，程序是使用虚拟地址的，所以配置DMA时必须将虚拟地址转化成物理地址。

2.因为程序使用虚拟地址，而且一般使用CACHED地址，所以虚拟地址中的内容与其物理地址上的内容不一定一致的，所以在启动DMA传输之前一定要将该地址的CACHE刷新，即写入内存。

3.OS并不能保证每次分配到的内在空间在物理上是连续的。尤其是在系统使用过一段时间而又分配了一块比较大的内存时。

所以每次都需要判断地址是不是连续的，如果不连续就需要把这段内存分成几段让DMA完成传输。