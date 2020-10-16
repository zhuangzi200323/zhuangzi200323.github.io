---
title: window mobile 5.0截获来电
date: 2008-03-18 15:06:15
img: '/medias/17.jpg'
categories:
- Windows Mobile
---

## window mobile 5.0截获来电

在开发来电防火墙或来电大头贴时，首先就是要截获来电，然后再加以分析，截获来电的方法一般就是用TAPI提供的api函数，这些函数在tapi.h下．
１．初始化线路，并打开线路
```CPP
DWORD lineHandleCount;
HLINE *lineHandles;
HLINEAPP lineApp;

if(lineInitialize(&lineApp,theApp.m_hInstance,(LINECALLBACK)LineCallback,L"Video",&lineHandleCount)==0)
{
     lineHandles = new HLINE[lineHandleCount];

        for(DWORD i = 0; i < lineHandleCount; i++)
        {

            int rc;
            DWORD ver;
            LINEEXTENSIONID extensionID;

            if( lineNegotiateAPIVersion( lineApp, i, 0x00010000, 0x00020000, &ver, &extensionID ) == 0 )
            {
               rc = lineOpen( lineApp,i,&lineHandles[ i ],ver,0,(DWORD)0,LINECALLPRIVILEGE_MONITOR|LINECALLPRIVILEGE_OWNER,LINEMEDIAMODE_INTERACTIVEVOICE,NULL);
            }
        }
}
```
２.编写回调函数，在这个函数里实现功能
```CPP
VOID CALLBACK LineCallback(DWORD hDevice, DWORD dwMsg, DWORD dwCallbackInstance, DWORD dwParam1, DWORD dwParam2, DWORD dwParam3)
{
　 //dwMsg是线路消息
   //dwParam1,dwParam2是具体的事件

switch (dwMsg)
{
   case LINE_CALLSTATE: // Sent after change of call state
      switch (dwParam1)
      {
         case LINECALLSTATE_CONNECTED:
            if(!IsVoiceCall((HCALL)hDevice))
               break;
                  if (IsIncomingCall((HCALL)hDevice))
                  {
          ...........
   }

break;
}
```
3.当结束程序时，关闭线路
```CPP
for ( DWORD i=0; i < lineHandleCount; i++ )
{
    lineClose( lineHandles[i]);
}
delete[] lineHandles;
lineShutdown( lineApp );
```
