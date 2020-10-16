---
title: Windows Mobile 6.0中函数SetActiveProfile()在Smart Phone中的使用方法介绍
date: 2008-01-23 15:06:15
img: '/medias/10.jpg'
categories:
- Windows Mobile
---

## Windows Mobile 6.0中函数SetActiveProfile()在Smart Phone中的使用方法介绍

1、首先安装AKU，在AKU的如下路径\Public\Shell\Oak\Inc有头文件iprofiles.h，使用SetActiveProfile()需包含这个头文件。例(我把AKU装在D盘)：
```bash
#include <D:\WM607\PUBLIC\SHELLW\OAK\INC\iprofiles.h>
```
2、定义UUID，如下：
```bash
struct __declspec(uuid("B7A5E609-3E46-442d-8D32-65926EDA6795")) ProfilesApp;
```
3、使用SetActiveProfile()这个函数，如下：
```CPP
ITPCProfiles* pProfiles = NULL;
HRESULT hr = CoInitializeEx(NULL, COINIT_MULTITHREADED);
hr = CoCreateInstance(__uuidof(ProfilesApp), NULL, CLSCTX_INPROC_SERVER, __uuidof(ITPCProfiles), (LPVOID*)&pProfiles);

if(SUCCEEDED(hr) && pProfiles != NULL)
{
   hr = pProfiles->SetActiveProfile(g_ActiveProfile, NULL, FALSE);
   pProfiles->Release();
   pProfiles = NULL;
}
```
**总结**：SetActiveProfile()是用来设置Smart Phone的Profiles模式。