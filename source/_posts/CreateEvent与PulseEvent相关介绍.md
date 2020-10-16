---
title: CreateEvent与PulseEvent相关介绍
date: 2008-01-26 15:06:15
img: '/medias/12.jpg'
categories:
- MFC
---


CreateEvent的用法
```CPP
HANDLE CreateEvent(
   LPSECURITY_ATTRIBUTES lpEventAttributes, // SD
   BOOL bManualReset,           // reset type
   BOOL bInitialState,           // initial state
   LPCTSTR lpName             // object name
);
```
该函数创建一个Event同步对象,并返回该对象的Handle

lpEventAttributes 一般为NULL
bManualReset创建的Event是自动复位还是人工复位，如果true，人工复位，一旦该Event被设置为有信号,则它一直会等到ResetEvent()API被调用时才会恢复为无信号.如果为false，Event被设置为有信号,则当有一个wait到它的Thread时，该Event就会自动复位，变成无信号。  
bInitialState初始状态，true有信号，false无信号。
lpName Event对象名。
    
一个Event被创建以后,可以用OpenEvent()API来获得它的Handle，用CloseHandle()来关闭它，用SetEvent()或PulseEvent()来设置它使其有信号,用ResetEvent()来使其无信号,用WaitForSingleObject()或waitForMultipleObjects()来等待其变为有信号。   
    
PulseEvent()是一个比较有意思的使用方法，正如这个API的名字，它使一个Event对象的状态发生一次脉冲变化，从无信号变成有信号再变成无信号，而整个操作是原子的。对自动复位的Event对象，它仅释放第一个等到该事件的thread（如果有)，而对于人工复位的Event对象,它释放所有等待的thread。