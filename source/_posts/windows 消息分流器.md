---
title: Windows消息分流器
date: 2008-07-04 15:06:15
img: '/medias/13.jpg'
categories:
- Windows
---



很好理解, windows 操作系统使用消息处理机制, 那么, 我们所设计的程序如何才能分辨和处理系统中的各种消息呢? 这就是消息分流器的作用.

简单来说, 消息分流器就是一段代码, 在我的讲述中, 将分 7 重来循序渐进的介绍它. 从最初的第 1 重到最成熟的第 7 重, 它的样子会有很大的变化. 但, 实现的功能都是一样的, 所不同的, 仅仅是变得更加简练罢了.

程序开始时候, 会是 main 函数, 然后会生成初始的窗口, 同时会调用 WndProc 函数. 这是一个自定义的函数, 名字也会有变化, 但其功能是一样的, 就是运行消息分流器. WndProc 函数如下:

LRESULT CALLBACK WndProc (HWND hwnd, UINT msg,WPARAM wParam, LPARAM lParam)  
{

//......

return DefWindowProc(hwnd, msg, wParam, lParam);

}

这其中, hwnd 是窗口的句柄, msg 是系统发送来的消息的名字. wParam 和 lParam 则是随消息一起发送来的消息参数.

WndProc 函数使用了消息分流器，下面把消息分流器的内容解释一下：

一重，当不同的消息出现时，在其中写入相应的程序语句即可。  
LRESULT CALLBACK WndProc (HWND hwnd, UINT msg,WPARAM wParam, LPARAM lParam)  
{  
  case WM\_CREATE:  
  // ...  
  return 0;

  case WM\_PAINT:  
  // ...  
  return 0;

  case WM\_DESTROY:  
  //...  
  return 0;  
 }  
 return DefWindowProc(hwnd, msg, wParam, lParam);  
}

二重，运用三个消息分流器进行处理。  
LRESULT CALLBACK WndProc (HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)  
{  
  case WM\_CREATE:  
  return HANDLE\_WM\_CREATE(hwnd, wParam, lParam, Cls\_OnCreate);

  case WM\_PAINT:  
  return HANDLE\_WM\_PAINT(hwnd, wParam, lParam, Cls\_OnPaint);

  case WM\_DESTROY:  
  return HANDLE\_WM\_DESTROY(hwnd, wParam, lParam, Cls\_OnDestroy);  
 }  
 return DefWindowProc(hwnd, msg, wParam, lParam);  
}  
这里的 HANDLE\_WM\_CREATE，HANDLE\_WM\_PAINT，HANDLE\_WM\_DESTROY 就是消息分流器。  
与消息不同之处就是在前面增加了 “HANDLE\_” 字符，windows 的消息分流器就是这样的模样。  
它的本质就是宏定义。  
其中的四个参数有三个都是从本函数的入口参数中直接得到的，即为 hwnd, wParam, lParam。  
只有第四的参数是表明调用的函数。  
消息分流器是在 winowsx.h 文件中定义的。由此，可以看出第四个参数是调用的函数，其定义如下：

#define HANDLE\_WM\_CREATE(hwnd, wParam, lParam, fn) ((fn)((hwnd), (LPCREATESTRUCT)(lParam)) ? 0L : (LRESULT)-1L)

#define HANDLE\_WM\_PAINT(hwnd, wParam, lParam, fn) ((fn)(hwnd), 0L)

#define HANDLE\_WM\_DESTROYCLIPBOARD(hwnd, wParam, lParam, fn) ((fn)(hwnd), 0L)

0L 是表示 int 类型的变量，其数值为 0。  
int 类型时，可在后面加 l 或者 L(小写和大写形式)  
表明无符号数时，可在后面加 u 或者 U(小写和大写形式)  
float 类型时，可在后面加 f 或者 F(小写和大写形式)  
例如：  
128u 1024UL 1L 8Lu 3.14159F 0.1f

LRESULT 是一个系统的数据类型，其定义如下：  
typedef LONG\_PTR LRESULT;

LONG\_PTR 也是一个系统的数据类型，其定义如下：  
#if defined(\_WIN64)  
 typedef \_\_int64 LONG\_PTR;  
#else  
 typedef long LONG\_PTR;  
#endif  
由此可见，LRESULT 的实质就是 64 的 long 类型的变量

那么 (LRESULT)-1L 的实质并不是减法，而是 ((LRESULT)(-1L))，即强制类型转换

三重，把消息分流器的宏定义代换回去，就成了下面的样子  
LRESULT CALLBACK WndProc (HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)  
{  
  case WM\_CREATE:  
  return Cls\_OnCreate(hwnd, (LPCREATESTRUCT)(lParam)) ? 0L : (LRESULT)-1L;  
  // 如果处理了消息，则 Cls\_OnCreate 应返回 TRUE，导致 WndProc 返回 0，否则 Cls\_OnCreate 返回 FALSE，导致 WndProc 返回 - 1；

  case WM\_PAINT:  
  return Cls\_OnPaint(hwnd), 0L;  
  // 逗号表达式；Cls\_OnPaint 是 void 类型，这里返回 0；

  case WM\_DESTROY:  
  return Cls\_OnDestroy(hwnd), 0L; // 同 Cls\_OnPaint  
 }  
 return DefWindowProc(hwnd, msg, wParam, lParam);  
}  
在逗号表达式，C++ 会计算每个表达式，但完整的逗号表达式的结果是最右边表达式的值。  
所以，会 return 0。  
然后，就可以手动的编写各个处理函数了：Cls\_OnCreate，Cls\_OnPaint，WM\_DESTROY。

四重，  
LRESULT CALLBACK WndProc (HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)  
{  
  HANDLE\_MSG(hwnd, WM\_CREATE, Cls\_OnCreate);  
  HANDLE\_MSG(hwnd, WM\_PAINT, Cls\_OnPaint);  
  HANDLE\_MSG(hwnd, WM\_DESTROY, Cls\_OnDestroy);  
 }  
 return DefWindowProc(hwnd, msg, wParam, lParam);  
}

HANDLE\_MSG 也是一个宏，它在 windowsx.h 中定义，如下：  
#define HANDLE\_MSG(hwnd, message, fn) case (message): return HANDLE\_##message((hwnd), (wParam), (lParam), (fn))

这个宏要做的就是根据不同的 message（## 用来连接前后的字符串），把自己 “变成” 相应的 HANDLE\_XXXXMESSAGE 形式的宏，再通过相应的宏来执行消息处理代码。  
说白了，就是把 message 的消息做为替换，## 就是一个替换的标志。  
如果没有 ##，就成了 HANDLE\_message 了，这样，宏是不会被代换的。  
如果就单独一个，则会代换，如 hwnd 和 fn。

比如实际代码中写入：  
HANDLE\_MSG(hwnd, WM\_CREATE, Cls\_OnCreate)  
则经过转换就变成：  
case (WM\_CREATE): return HANDLE\_WM\_CREATE((hwnd), (wParam), (lParam), (Cls\_OnCreate))  
这与二重一模一样。

以上四重，是消息分离器的基本使用，但，这不完整，消息分离器主要应用在对话框消息处理中。  
这里，窗口子类化是我们经常使用的手段，这也可以通过消息分流器实现，

第五重  
LRESULT CALLBACK Dlg\_Proc (HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)  
{  
  HANDLE\_MSG(hwnd, WM\_INITDIALO , Cls\_OnInitDialog); // 不能直接使用 HANDLE\_MSG 宏  
  HANDLE\_MSG(hwnd, WM\_COMMAND, Cls\_OnCommand); // 不能直接使用 HANDLE\_MSG 宏  
 }  
 return false;  
}  
由于是窗口子类化，所以，最后，返回的是 false，以表明，如果没有约定响应的消息，  
则返回父亲窗口 false，如果有，则返回 ture，这是与前四重不同的地方。  
一般情况下，对话框过程函数应该在处理了消息的情况下返回 TRUE，如果没有处理，则返回 FALSE。  
如果对话框过程返回了 FALSE，那么对话框管理器为这条消息准备默认的对话操作。

但是，这其中有错误，因为有的消息, 需要单独处理。单独处理的消息列表见 SetDlgMsgResult 宏。

第六重  
这点小问题，这就需要用到 SetDlgMsgResult(hwnd, msg, result) 宏。

LRESULT CALLBACK Dlg\_Proc (HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)  
{  
 case WM\_INITDIALO:  
 return (SetDlgMsgResult(hwnd, Msg, HANDLE\_WM\_INITDIALO((hwnd), (wParam), (lParam), (fn)));

 case WM\_COMMAND:  
 return (SetDlgMsgResult(hwnd, Msg, HANDLE\_WM\_COMMAND((hwnd), (wParam), (lParam), (fn)));  
 }  
return false;  
}  
这里，就用直接用到了第二重的消息分流器，而抛弃了其他。

这个宏定义如下：  
#define SetDlgMsgResult(hwnd, msg, result)  
(  
 (  
 (msg) == WM\_CTLCOLORMSGBOX ||  
 (msg) == WM\_CTLCOLOREDIT ||  
 (msg) == WM\_CTLCOLORLISTBOX ||  
 (msg) == WM\_CTLCOLORBTN ||  
 (msg) == WM\_CTLCOLORDLG ||  
 (msg) == WM\_CTLCOLORSCROLLBAR ||  
 (msg) == WM\_CTLCOLORSTATIC ||  
 (msg) == WM\_COMPAREITEM ||  
 (msg) == WM\_VKEYTOITEM ||  
 (msg) == WM\_CHARTOITEM ||  
 (msg) == WM\_QUERYDRAGICON ||  
 (msg) == WM\_INITDIALOG  
 ) ?  
 (BOOL)(result) :  
 (SetWindowLongPtr((hwnd), DWLP\_MSGRESULT, (LPARAM)(LRESULT)(result)), TRUE)  
)

为了表述清楚，所以用了此格式，这是一个三项表达式，首先对消息类型进行考察。

如果对话框过程处理的消息恰巧为返回特定值中的一个，则如实返回 result；  
不要被前面的 BOOL 蒙蔽，BOOL 在头文件中的定义实际上是一个 int 型，  
一旦需要返回非 TRUE 或 FALSE 的其他值，照样可以；

这样，我们的 Cls\_OnInitDialog 就能够正确的返回它的 BOOL 值了，  
而 Cls\_OnCommand 在处理之后，也可以由后面的逗号表达式正确的返回一个 TRUE 表示消息已处理。

第七重  
我们还可以把 case 也包含进来，就成了如下的样子。

LRESULT CALLBACK Dlg\_Proc (HWND hwnd, UINT msg, WPARAM wParam, LPARAM lParam)  
{  
 chHANDLE\_DLGMSG(hwnd, WM\_INITDIALOG, Cls\_OnInitDialog);  
 chHANDLE\_DLGMSG(hwnd, WM\_COMMAND, Cls\_OnCommand);  
 }  
 return false;  
}

chHANDLE\_DLGMSG 是牛人定义的一个宏，它把 case 也包含进来了。  
#define chHANDLE\_DLGMSG(hwnd, message, fn) case (message): return (SetDlgMsgResult(hwnd, uMsg, HANDLE\_##message((hwnd), (wParam), (lParam), (fn))))

这样，程序中的语句  
 switch (uMsg)  
 {  
  chHANDLE\_DLGMSG(hwnd, WM\_INITDIALOG, Dlg\_OnInitDialog);  
  chHANDLE\_DLGMSG(hwnd, WM\_SIZE,       Dlg\_OnSize);  
  chHANDLE\_DLGMSG(hwnd, WM\_COMMAND,    Dlg\_OnCommand);  
 }

就被翻译成：  
 switch (uMsg)  
 {  
 case (WM\_INITDIALOG):  
  return (SetDlgMsgResult(hwnd, uMsg, HANDLE\_WM\_INITDIALOG((hwnd), (wParam), (lParam), (Dlg\_OnInitDialog))));

 case (WM\_SIZE)  
  return (SetDlgMsgResult(hwnd, uMsg, HANDLE\_WM\_SIZE((hwnd), (wParam), (lParam), (Dlg\_OnSize))));

 case (WM\_COMMAND)  
  return (SetDlgMsgResult(hwnd, uMsg, HANDLE\_WM\_COMMAND((hwnd), (wParam), (lParam), (Dlg\_OnCommand))));  
 }  

这样, 消息分流器, 就介绍完毕.

原文地址 \[blog.csdn.net\](https://blog.csdn.net/hopkins9961629/article/details/588184)