---
title: 中断使能/禁止－inUnlock()与intLock()
date: 2008-03-18 15:06:15
img: '/medias/16.jpg'
categories:
- API
tags:
- VxWorks
---

## 中断使能/禁止－inUnlock()与intLock()

VxWorks提供两个重要API：

（1）intLock()：使中断禁止

（2）intUnlock()：开中断

可以用intLock/intUnlock提供最高级别的互斥机制以保护临界区域不被打断，例如：

oldlevel = intLock();

/* 写XXX寄存器 */

XXX_REG_WRITE(pChan, XXX_UBRDIV, XXX_CNT0_115200 | XXX_CNT1_VAL);

intUnlock(oldlevel);

用intLock()禁止中断后，当前执行的任务将一直继续，中断处理和任务调度得不到执行，直到该任务主动调用intUnLock解锁中断为止。对于intLock和unLock的使用，我们要注意如下几点：

（1）不要在中断禁止期间调用vxWorks系统函数，否则有可能意外使能中断，违反临界代码的设计意图。另外，intLock也不能屏蔽调度，如果在中断禁止代码区使用系统调用，就可能出现任务调度，其他任务的运行可能会解锁中断；

（2）中断禁止对系统的实时性有很大的影响，在解决执行代码和中断处理互斥问题才可使用，并且应使中断禁止时间尽可能的短。对于任务间的互斥问题，可以使用taskLock()和taskUnLock()来解决；

（3）有些CPU中断是分级，我们可以用intLockLevelSet()和intLockLevelGet()来操作中断闭锁的级别。缺省情况下，taskLock禁止所有等级的中断。

至此，我们可以对“互斥”问题进行一个系统的总结，主要有如下几种方法：

（1）intLock禁止中断：解决任务和ISR之间的互斥问题；

int lock = intLock();

//. . critical region that cannot be interrupted

intUnlock(lock);

（2）taskLock禁止优先级抢占调度：当当前任务正在运行时，除了中断服务程序外，高优先级的任务也不允许抢占CPU；

taskLock();

//. . critical region that cannot be interrupted .

taskUnlock();

（3）二进制信号量或互斥信号量。

semTake (semMutex, WAIT_FOREVER);

//. . critical region, only accessible by a single task at a time .

semGive (semMutex);

总的来说，在实时系统中采取“禁止中断”的方法会影响系统对外部中断及时响应和处理的能力；而“禁止优先级抢占调度”方法阻止了高优先级的任务抢先运行，在实时系统中也是不适合的。因此，信号量无疑是解决互斥问题的最好方法。