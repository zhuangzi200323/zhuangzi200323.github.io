---
title: $_FILES
date: 2009-09-17 15:06:15
img: '/medias/16.jpg'
categories:
- PHP
---

## $_FILES
经由 HTTP POST 文件上传而提交至脚本的变量。类似于旧数组 $HTTP_POST_FILES 数组（依然有效，但反对使用）。详细信息请参阅 POST 方法上传。

$_FILES数组内容如下:
```php
<input type="file" name="userfile">
```
```php
$_FILES['userfile']['name']
```

客户端机器文件的原名称。
```php
$_FILES['userfile']['type']
```
文件的 MIME 类型，需要浏览器提供该信息的支持，例如“image/gif”。
```php
$_FILES['userfile']['size']
```
已上传文件的大小，单位为字节。
```php
$_FILES['userfile']['tmp_name']
```
文件被上传后在服务端储存的临时文件名。
```php
$_FILES['userfile']['error']
```
和该文件上传相关的错误代码。['error'] 是在 PHP 4.2.0 版本中增加的。

**注:** 在 PHP 4.1.0 版本以前该数组的名称为 $HTTP_POST_FILES，它并不像 $_FILES 一样是自动全局变量。PHP 3 不支持 $HTTP_POST_FILES 数组。