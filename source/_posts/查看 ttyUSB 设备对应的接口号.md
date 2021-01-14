---
title: 查看 ttyUSB 设备对应的接口号
date: 2020-12-21 15:06:15
img: '/medias/13.jpg'
categories:
- Android
---

## 查看 ttyUSB 设备对应的接口号

  在设备开发中，常用到基于 USB 的虚拟串口，此类接口在 linux 平台上被枚举为 ttyUSB 设备。  

当有多个 ttyUSB 设备时，常常导致分不清是哪个功能接口。

  其实还是可以很容易分出来的，在 sysfs 中通过以下步骤查看可以轻而易举的搞得一清二楚。

  查看 sys 目录，会看到 bus 子目录

```bash
/#: ls /sys
block     class     devices   firmware  kernel    pmu       zte
bus       dev       dwc_usb   fs        module    power

```

  查看 bus 子目录，会有 USB 子目录  

```bash
/#: ls /sys/bus
clocksource  i2c          rpmsg_zx29   serio        usb
cpu          platform     scsi         spi          usb-serial

```

  查看 USB 子目录，会有 devices 和 drivers 子目录

```bash
/ # ls sys/bus/usb
devices            drivers_autoprobe  uevent
drivers            drivers_probe

```

  在 devices 目录下，即是所有的 USB 设备  

```bash
/ # ls sys/bus/usb/devices
1-0:1.0  1-1      1-1:1.0  1-1:1.1  1-1:1.2  1-1:1.3  1-1:1.4  1-1:1.6  usb1

```

  任选一个查看，比如选择 1-1:1.2, 可以看到该设备的诸多属性

```bash
/ # ls sys/bus/usb/devices/1-1:1.2
bAlternateSetting     driver                subsystem
bInterfaceClass       ep_03                 supports_autosuspend
bInterfaceNumber      ep_83                 ttyUSB0
bInterfaceProtocol    interface             uevent
bInterfaceSubClass    modalias
bNumEndpoints         power

```

  同时可以看到，它所对应的 ttyUSB 设备是 ttyUSB0，但是这个是哪个接口，是什么功能呢？

其实 1-1:1.2 这个名字已经表明了是接口 2，但是对于普通人来说很难去记，还是进一步查看确认一下才好。查看接口号：

 bInterfaceNumber 代表接口号，可以看到是 02

```bash
/ # cat sys/bus/usb/devices/1-1:1.2/bInterfaceNumber
02

```

查看对应的功能接口描述，interface 代表功能接口描述，可以看到是 USB-AT，表明是 AT 口

```bash
/ # cat sys/bus/usb/devices/1-1:1.2/interface
USB-AT

```

由此可知，ttyUSB0 对应的是 AT 口。  

其他的所有接口也可以通过该方法查看。

转自：https://blog.51cto.com/tianya30/1842264