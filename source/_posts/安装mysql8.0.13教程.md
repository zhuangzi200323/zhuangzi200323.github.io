---
title: 安装mysql8.0.13教程
date: 2019-06-02 15:06:15
img: '/medias/9.png'
categories:
- spring
tags:
- MySql
---

## 安装mysql8.0.13教程

### 1. 下载

<https://dev.mysql.com/downloads/mysql/>

![](media/6a6e692c26ad90557f4e555b6cd18784.jpeg)

### 2. 解压到指定目录

![](media/bf9d791d234c3fbd3c285cbf509bc8b6.jpeg)

### 3. 在D:\\mysql-8.0.13-winx64目录下添加my.ini文件

[mysql]

\# 设置mysql客户端默认字符集

```bash
default-character-set=utf8
```
[mysqld]

\# 设置3306端口

```bash
port = 3306
```

\# 设置mysql的安装目录

```bash
basedir=D:\\\\mysql-8.0.13-winx64
```

\# 设置 mysql数据库的数据的存放目录，MySQL 8+
不需要以下配置，系统自己生成即可，否则有可能报错
```bash
# datadir=C:\\\\web\\\\sqldata
```
\# 允许最大连接数
```bash
max_connections=20
````
\# 服务端使用的字符集默认为8比特编码的latin1字符集
```bash
character-set-server=utf8
```
\# 创建新表时将使用的默认存储引擎
```bash
default-storage-engine=INNODB
```
### 4. 添加环境变量

![](media/e5c325b33c12a8cd40bb2402a0645235.jpeg)

### 5. 安装mysql

**mysqld --initialize –console**

![](media/4724c156dbb920f958697f996f209bfb.png)

注意！[注意] [MY-010454] [服务器]为root \@
localhost生成临时密码：9P0gYk-？0，kT其中root \@
localhost：后面的9P0gYk-？0，kT就是初始密码（不含首位空格）。在没有更改密码前，需要记住这个密码，后续登录需要用到。复制密码先保存起来!!!

### 6. 安装mysql服务及启动mysql服务

![](media/7977ad0c14cec5ad2475f19c1de5fa72.jpeg)

### 7. 修改密码

![](media/7b61820855d32ed0a63a68db37a3e016.jpeg)

### 8. 使用navicat连接时提示如下错误解决办法

![](media/1e58ecf5c299258833a7936dfb2c073e.jpeg)

![](media/409504c90f97dde0081b03c62da6c09e.jpeg)

转自：<http://www.runoob.com/mysql/mysql-install.html>
