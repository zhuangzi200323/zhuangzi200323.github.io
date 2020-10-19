---
title: Windows CE 的电源管理之三
date: 2008-03-21 15:06:15
img: '/medias/8.jpg'
categories:
- WinCE
---

## Windows CE的电源管理之三

本篇将以Windows Mobile为例介绍Windows CE电源管理的实现，大体上，Windows
Mobile分为Pocket
PC和Smartphone两种版本。这两者之间的主要区别在于触摸屏和电源模型，Smartphone采用的是“Always
On”模型。为了说清楚它们的区别，我们就先从系统电源状态说起吧（这里有些系统电源状态是从WM5开始才有的）。

**1. Windows Mobile的系统电源状态**

-   On：用户与系统交互时的状态；

-   BacklightOff：在一段时间内（默认15秒），如果一直没有用户操作（比如按下某个键或者触摸屏幕），就关闭背光，这时其他的设备都没变化。这个timeout值可以通过控制面板进行设置；

-   UserIdle：这个状态只在Smartphone中被使用。经过一段稍长的时间，如果一直没有用户操作，就关闭背光和LCD。这个timeout值可以通过控制面板进行设置；

-   ScreenOff：一般由某些程序指定，才进入这个状态。比如音乐播放器程序，当你听音乐时按下某个键可以将屏幕关闭。PocketPC和Smartphone都使用这个状态，它与UserIdle的不同在于，ScreenOff意味着“用户主动关闭了显示，只有当他按下电源键时才重新显示”，而UserIdle意味着“用户有段时间没操作了，那么我们可以关闭屏幕来省电”，所以在UserIdle时，随便按下Smartphone的哪个键都会启动显示；

-   Suspend：这是PocketPC的睡眠模式，几乎所有设备都被关闭，直到某个硬件设备触发中断才将系统唤醒，这个timeout值可以通过控制面板进行设置（默认为3分钟）；

-   Resuming：这是PocketPC被唤醒后的状态，这时屏幕是关闭的，并启动一个15秒的计时器，在这段时间内决定接下来进入哪个状态，如果计时器超时则重新回到睡眠状态；

-   Unattended：这个状态只在PocketPC中被使用，用户对其不会有所察觉。有些程序，如ActiveSync每5分钟会唤醒系统进行同步，同步完成后再让系统继续睡眠，这段时间不希望打扰用户，即程序在后台执行。

可以通过注册表查看系统电源状态对应的具体设备的电源状态，[HLM\\System\\CurrentControlSet\\Control\\Power\\State]。

现在我们知道，Smartphone没有真正的睡眠模式，即使它会在一段时间后关闭背光和屏幕，但它并没有睡着，只是休息一下眼睛罢了，它的大脑和四肢仍在正常工作。PocketPC所采用的模型比Smartphone要复杂的多，你可以按下电源键让系统睡眠，在必要时，也可以唤醒系统做一些工作然后再继续睡眠。如果你在Smartphone上运行一个桌面精灵之类的程序，她为了引起你的注意，长时间的蹦啊跳啊，不管白天还是黑夜，可想而知，你的待机时间将......

你可能会觉得PocketPC的“Sleep”模型比Smartphone的“Always
On”模型要省电，其实恰恰相反。因为在系统睡眠的过程中，它需要通知所有的设备驱动，为了让它们保存一些重要的信息并关闭相应的硬件设备，在系统被唤醒时也需要通知它们恢复先前的工作。这个过程不仅耗时还可能会耗更多的电，因为一些设备在频繁的状态转换过程中会消耗比较多的能量。这也就是为什么当你收到一条短信时，睡眠状态的PocketPC要花3到6秒的时间来处理，而Smartphone只需要几个微秒:)

2. Windows Mobile的电源管理策略

我们可以用系统电源状态机来简单的描述Windows
Mobile的电源管理策略，以PocketPC为例，系统电源状态机如下图所示：

![7a7e0cdc54b.jpg](media/332f42f13ae5217dc7b4c7a7e0cdc54b.jpg)

系统内部的电源管理器负责协调电源状态的转换，电源状态的转换主要由一下几种方式触发：

-   计时器超时：SuspendTimeout和ResumingSuspendTimeout，分别对应于第一节介绍Suspend和Resuming状态时所提到的计时器。细说起来，它们每个又有两个值，分别对应着电源供电时和电池供电时的超时值，也就是注册表[HLM\\System\\CurrentControlSet\\Control\\Power\\Timeout]中的ACSuspendTimeout、BattSuspendTimeout、ACResumingSuspendTimeout、BattResumingSuspendTimeout；

-   系统调用：驱动程序或应用程序通过相应的API，请求进入某种电源状态。这类API在前面的文章中已经有所介绍，如SetSystemPowerState、SetPowerRequirement、DevicePowerNotify等；

-   平台相关的系统调用：通过PowerPolicyNotify通知电源管理器发生了某个事件，它的实现比较灵活，驱动程序或应用程序可以通过相应的参数与电源管理器进行交互，比如PPN_POWERCHANGE、PPN_SUSPENDKEYPRESSED、PPN_UNATTENDEDMODE等，参见"pmpolicy.h"；

-   直接访问内核对象：事件（Event）作为Windows
    CE系统的内核对象，可以通过事件名称在进程间共享，因此我们可以访问电源管理器中的两个事件，它们的名字分别是_T("PowerManager/ReloadActivityTimeouts")、_T("PowerManager/SystemIdleTimerReset")。如果你的程序需要动态修改那几个计时器的时间长度，可以通过第一个事件通知电源管理器重新读取注册表中计时器的值，而第二个事件与SystemIdleTimerReset功能一样，可以阻止系统进入睡眠状态。

3. Windows Mobile电源管理相关API的应用

最后，通过几个应用场景简单介绍一下常用的电源管理相关的API的使用：

-   如果你在设计的是媒体播放器程序，不希望在播放电影时，系统自动转入Suspend状态，这时可以每隔30秒调用一次SystemIdleTimerReset，它会帮你重置那个计时器；如果你还想同时保持背光，那么可以调用SetPowerRequirement(TEXT("BKL1:"),
    D0, POWER_NAME, NULL,
    0);如果你提供一个按钮允许用户关闭屏幕，那么调用SetSystemPowerState(NULL,
    POWER_STATE_IDLE, 0);

-   如果你在设计的是天气预报程序，需要每天早上6点在线更新天气信息，这时可以调用CeRunAppAtTime，系统到时会被RTC中断唤醒，还记得前面提到的那个15秒的计时器吗，这时你的程序应该在15秒内请求进入Unattended状态，否则系统将重新回到睡眠状态。在处理更新的过程中，还是应该每隔30秒调用一次SystemIdleTimerReset，在处理完更新后，应该再次调用CeRunAppAtTime，并放弃Unattended状态。请注意，在电源管理器的实现代码中，用了一个引用计数的变量（gdwUnattendedModeRequests）统计所有对Unattended状态的请求，所以PowerPolicyNotify(PPN_UNATTENDEDMODE,
    TRUE);和PowerPolicyNotify(PPN_UNATTENDEDMODE,
    FALSE);要成对出现，否则系统将无法回到睡眠状态。

-   如果你要开发一个监控电池状态的程序，首先应该创建一个接收状态通知的线程，在这个线程里调用RequestPowerNotifications，这个函数的第一个参数是一个消息队列的句柄，所以必须先创建一个消息队列（CreateMsgQueue），第二个参数是你希望得到的通知类型，这里要用到的是PBT_POWERSTATUSCHANGE\|PBT_POWERINFOCHANGE，然后线程就可以等待通知了（WaitForSingleObject），一旦有通知到来，线程通过ReadMsgQueue读取消息的内容，再做些更新UI的工作。

相关示例代码
``` CPP
//***************************************************************************
// Function Name: PowerNotificationThread
//
// Purpose: listens for power change notifications
//
DWORD PowerNotificationThread(LPVOID pVoid)
{
    // size of a POWER_BROADCAST message
    DWORD cbPowerMsgSize = sizeof POWER_BROADCAST + (MAX_PATH * sizeof TCHAR);

    // Initialize our MSGQUEUEOPTIONS structure
    MSGQUEUEOPTIONS mqo;
    mqo.dwSize = sizeof(MSGQUEUEOPTIONS);
    mqo.dwFlags = MSGQUEUE_NOPRECOMMIT;
    mqo.dwMaxMessages = 4;
    mqo.cbMaxMessage = cbPowerMsgSize;
    mqo.bReadAccess = TRUE;             
                                        
    // Create a message queue to receive power notifications
    HANDLE hPowerMsgQ = CreateMsgQueue(NULL, &mqo);
    if (NULL == hPowerMsgQ)
    {
        RETAILMSG(1, (L"CreateMsgQueue failed: %x\n", GetLastError()));
        goto Error;
    }

    // Request power notifications
    HANDLE hPowerNotifications = RequestPowerNotifications(hPowerMsgQ, PBT_POWERSTATUSCHANGE | PBT_POWERINFOCHANGE);
   
    if (NULL == hPowerNotifications)
    {
        RETAILMSG(1, (L"RequestPowerNotifications failed: %x\n", GetLastError()));
        goto Error;
    }

    HANDLE rgHandles[2] = {0};
    rgHandles[0] = hPowerMsgQ;
    rgHandles[1] = g_hEventShutDown;

    // Wait for a power notification or for the app to exit
    while(WaitForMultipleObjects(2, rgHandles, FALSE, INFINITE) == WAIT_OBJECT_0)
    {
        DWORD cbRead;
        DWORD dwFlags;
        POWER_BROADCAST *ppb = (POWER_BROADCAST*) new BYTE[cbPowerMsgSize];
           
        // loop through in case there is more than 1 msg
        while(ReadMsgQueue(hPowerMsgQ, ppb, cbPowerMsgSize, &cbRead,
                           0, &dwFlags))
        {
            switch (ppb->Message)
            {
                case PBT_POWERINFOCHANGE:
                {
                    RETAILMSG(1,(L"Power Notification Message: PBT_POWERINFOCHANGE\n"));

                    // PBT_POWERINFOCHANGE message embeds a
                    // POWER_BROADCAST_POWER_INFO structure into the
                    // SystemPowerState field
                    PPOWER_BROADCAST_POWER_INFO ppbpi =
                        (PPOWER_BROADCAST_POWER_INFO) ppb->SystemPowerState;
                    if (ppbpi)
                    {
                        RETAILMSG(1,(L"Length: %d", ppb->Length));
                        RETAILMSG(1,(L"BatteryLifeTime = %d\n",ppbpi->dwBatteryLifeTime));
                        RETAILMSG(1,(L"BatterFullLifeTime = %d\n",
                                     ppbpi->dwBatteryFullLifeTime));
                        RETAILMSG(1,(L"BackupBatteryLifeTime = %d\n",
                                     ppbpi->dwBackupBatteryLifeTime));
                        RETAILMSG(1,(L"BackupBatteryFullLifeTime = %d\n",
                                     ppbpi->dwBackupBatteryFullLifeTime));
                        RETAILMSG(1,(L"ACLineStatus = %d\n",ppbpi->bACLineStatus));
                        RETAILMSG(1,(L"BatteryFlag = %d\n",ppbpi->bBatteryFlag));
                        RETAILMSG(1,(L"BatteryLifePercent = %d\n",
                                     ppbpi->bBatteryLifePercent));
                        RETAILMSG(1,(L"BackupBatteryFlag = %d\n",
                                     ppbpi->bBackupBatteryFlag));
                        RETAILMSG(1,(L"BackupBatteryLifePercent = %d\n",
                                     ppbpi->bBackupBatteryLifePercent));
                    }
                    break;
                }

                default:
                    break;
            }

            UpdateUI();
        }

        delete[] ppb;
    }

Error:
    if (hPowerNotifications)
        StopPowerNotifications(hPowerNotifications);

    if (hPowerMsgQ)
        CloseMsgQueue(hPowerMsgQ);

    return NULL;
}
```
