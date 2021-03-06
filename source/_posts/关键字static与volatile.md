---
title: 关键字static/volatile
date: 2008-03-01 15:06:15
img: '/medias/13.jpg'
categories:
- C++
---

## 关键字static/volatile

### A.关键字static的作用是什么？

这个简单的问题很少有人能回答完全。在C语言中，关键字static有三个明显的作用：

1、在函数体，一个被声明为静态的变量在这一函数被调用过程中维持其值不变。

2、在模块内（但在函数体外），一个被声明为静态的变量可以被模块内所用函数访问，但不能被模块外其它函数访问。它是一个本地的全局变量。

3、在模块内，一个被声明为静态的函数只可被这一模块内的其它函数调用。那就是，这个函数被限制在声明它的模块的本地范围内使用。（大多数应试者能正确回答第一部分，一部分能正确回答第二部分，同是很少的人能懂得第三部分。这是一个应试者的严重的缺点，因为他显然不懂得本地化数据和代码范围的好处和重要性。）

### B.关键字volatile有什么含意?并给出三个不同的例子。

一个定义为volatile的变量是说这变量可能会被意想不到地改变，这样，编译器就不会去假设这个变量的值了。精确地说就是，优化器在用到这个变量时必须每次都小心地重新读取这个变量的值，而不是使用保存在寄存器里的备份。

**下面是volatile变量的几个例子：**

1、并行设备的硬件寄存器（如：状态寄存器）
2、一个中断服务子程序中会访问到的非自动变量(Non-automatic variables)
3、多线程应用中被几个任务共享的变量

回答不出这个问题的人是不会被雇佣的。我认为这是区分C程序员和嵌入式程序员的最基本的问题。搞嵌入式的家伙们经常同硬件、中断、RTOS等等打交道，所有这些都要求用到volatile变量。不懂得volatile的内容将会带来灾难。

假设被面试者正确地回答了这是问题（嗯，怀疑是否会是这样），我将稍微深究一下，看一下这家伙是不是直正懂得volatile完全的重要性。

1、一个参数既可以是const还可以是volatile吗？解释为什么。

2、一个指针可以是volatile吗？解释为什么。

下面的函数有什么错误：
```CPP
int square(volatile int *ptr)
{
    eturn *ptr * *ptr;
}
```

下面是答案：

1、是的。一个例子是只读的状态寄存器。它是volatile因为它可能被意想不到地改变。它是const因为程序不应该试图去修改它。

2、是的。尽管这并不很常见。一个例子是当一个服务子程序修该一个指向一个buffer的指针时。

这段代码有点变态。这段代码的目的是用来返指针\*ptr指向值的平方，但是，由于\*ptr指向一个volatile型参数，编译器将产生类似下面的代码：
```CPP
int square(volatile int *ptr)
{
	int a,b;

	a = *ptr;

	b = *ptr;

	return a * b;
}
```
由于\*ptr的值可能被意想不到地该变，因此a和b可能是不同的。结果，这段代码可能返回不是你所期望的平方值！正确的代码如下：
```CPP
long square(volatile int *ptr)
{
	int a;

	a = *ptr;

	return a * a;
}
```