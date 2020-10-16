---
title: 使用git命令时，出现diffie-hellman-group1-sha1错误处理方法
date: 2016-05-22 15:06:15
img: '/medias/4.jpg'
categories:
- Git相关
tags: 
- Git
---

使用git命令时，出现下方错误时
``` bash
Unable to negotiate with 192.168.4.28 port 19428: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1
fatal: Could not read from remote repository.
 
Please make sure you have the correct access rights
and the repository exists.
```
解决办法：
修改ssh config文件
``` bash
$ sudo gedit /etc/ssh/ssh_config 
```

在host下加入 
``` bash
KexAlgorithms=+diffie-hellman-group1-sha1
```

 
转自：http://blog.csdn.net/shadow066/article/details/51222577