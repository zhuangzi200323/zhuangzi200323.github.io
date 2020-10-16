---
title: CString::GetBuffer
date: 2007-09-20 15:06:15
img: '/medias/9.jpg'
categories:
- MFC
---


## CString::GetBuffer

对一个CString变量，你可以使用的唯一合法转换符是LPCTSTR。转换成LPTSTR（非常量指针）是错误的。养成把一个CString变量转换成LPTSTR的习惯将会给你带来伤害，因为当你的程序后来崩溃时，你可能不知道为什么，因为你到处都使用同样的代码而那时它们都恰巧正常工作。

正确的得到一个指向缓冲区的非常量指针的方法是调用GetBuffer()方法。

下面是正确的用法的一个例子，这段代码是给一个列表控件中的项设定文字：
```CPP
CString str = _T("new text");   
LVITEM item = {0};   
item.mask = LVIF_TEXT;   
item.iItem = 1;   
item.pszText = (LPTSTR)(LPCTSTR) str; // WRONG!   
item.pszText = str.GetBuffer(0);   // correct   
  
ListView_SetItem (&item);   
str.ReleaseBuffer();   // return control of the buffer to str
```

pszText成员是一个LPTSTR变量，一个非常量指针，因此你需要对str调用GetBuffer()。GetBuffer()的参数是你需要CString为缓冲区分配的最小长度。如果因为某些原因，你需要一个可修改的缓冲区来存放1K TCHARs，你需要调用GetBuffer(1024)。把0作为参数时，GetBuffer()返回的是指向字符串当前内容的指针。

getbuffer是为了让你使用CString类中,保存字符串缓冲区的那块指针。至于releasebuffer，在MSDN中有这样一句话.`
If you use the pointer returned by GetBuffer to change the string contents, you must call ReleaseBuffer before using any other CString member functions.   
`
在对GetBuffer返回的指针使用之后需要调用ReleaseBuffer，这样才能使用其他Cstring的operations。否则会发生错误。   

GetBuffer可以获得一个字符数组让你能够方便的对其进行操作，调用GetBuffer以后必须调用ReleaseBuffer释放，否则不能对该CString对象进行操作譬如   
```CPP
CString str = "abcd"   
char * lpBuf = str.GetBuffer(0);   
lpBuf[1] = 'e';   
int nLen = str.GetLength(); // 出错，ReleaseBuffer前不能进行类似的操作   
str.ReleaseBuffer(); // 此时str="aecd"
```