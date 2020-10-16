---
title: 无法加载文件 CUsers10186AppDataRoamingnpmhexo.ps1，因为在此系统上禁止运行脚本
date: 2020-07-22 15:06:15
img: '/medias/1.jpg'
categories:
- 问题记录
tags: 
- Hexo
---

**无法加载文件 C:\Users\10186\AppData\Roaming\npm\hexo.ps1，因为在此系统上禁止运行脚本
C:\\Users\\10186\\AppData\\Roaming\\npm\\hexo.ps1，因为在此系统上禁止运行脚本**

无论是npm run dev 还是 运行 hexo
s，在运行终端命令是会报错，发现提示错误信息"无法加载文件
C:\\Users\\10186\\AppData\\Roaming\\npm\\hexo.ps1，因为在此系统上禁止运行脚本。

![7678b62ce8.png](7f73d2e1b905a222464dda7678b62ce8.png)

解决方法：

1.  按下win+s输入powershell。然后右键以管理员身份运行。

![721092327687.png](d265dd3790349ba151b44efe45628f9a.png)

1.  然后输入set-ExecutionPolicy RemoteSigned命令，

2.  输入y，执行回车。

![721092529710.png](c726ada43233206372ec6151907be4c9.png)

1.  再次运行。

![721092554341.png](dea86e4a09f12a8ecc89b4734d108b7d.png)

1.  成功！

转自<https://blog.csdn.net/Dan_Xiao_Hui/article/details/107480800>
