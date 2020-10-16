---
title: C语言经典题目
date: 2008-03-18 15:06:15
img: '/medias/3.jpg'
categories:
- 面试
---


## C语言经典题目
int a ; int b;
### 1. 现在要求不使用临时缓冲区，交换a 和b的内容？
答：有两种比较常用的方法
方法一
```bash
a=a+b;
b=a-b;
a=a-b;
```
方法二
```bash
a=a^b;
b=a^b;
a=a^b;
```

### 2. 写一个函数fn()，功能是传一个参数是字符串，查找里面有没有相同的字符，有的话返回1，否则0。
两种方法,一是效率最高的,二是最节省内存的.
答：
```CPP
int fn1(const char *str)
{
	unsigned char *p = ( unsigned char *)str;
	int a[255]={0};
	for( ; *p != '\0'; p++ )
	   if( ++a[*p] == 2 ) return 1;
	return 0;
}
```
```CPP
int fn2(const char *str)
{
   const char *p1, *p2;
   if (*str == '\0')
      return 0;
   for (p1 = str; *p1 != ''; p1++)
      for (p2 = p1 + 1; *p2 !=''; p2++)
         if (*p1 == *p2)
            return 1;
   return 0;
}
```

### 3. 请参考整型变量i与0值做比较的判断条件（if（i==0）和if（i！=0））的写法写出以下判断
布尔值flag与0
float型与0
char *p 与0
答：
尔值flag与0

标准答案：    if ( flag )    if ( !flag )

float型与0

const float EPSINON = 0.000001;
if ((x >= - EPSINON) && (x <= EPSINON)

str \*p 与0

标准答案：    if (p == NULL)    if (p != NULL)

### 4. 如何用<<,>>,|,&实现一个WORD(2个字节)的高低位交换？

int test;
test = ( test<<8 & 0xFF00 ) | ( test>>8 & 0x00FF );

### 5. 已知链表头节点元素为head，指向下一节点的成员为head.next;要求不使用临时节点元素写出反序链表的C代码。

```CPP
void Invert(struct node *p)
{
     if(p->next==NULL) return;
     if(p->next->next!=0)
          Invert(p->next);
    p->next->next = p;
    p->next = 0;
}
```

### 6. 如何判断一个数是否是2的幂方？
```bash  
#define Is2n(a) ( a > 0 ) && ( ( ( ~a + 0x01 ) &a ) == a )
```

### 7. 有一百个整数，其中有负数，找出连续三个数之和最大的部分.
答：
但这种算法跟用最直接的方法来做的效率比并没有多大差异，仅仅提供一种思路。
```CPP

void main()
{
    int a[10]={-3,4,6,8,-9,7,10,-6,20,-9};
    int sum=a[0]+a[1]+a[2];//和
    int index=0; //该行的最小下标
    int d=0; //递增值

	for(int i=3;i<10;++i)
	{
		 d=d+a[i]-a[i-3];
		 if(d>0)
		{
		  sum = sum + d;
		  index=i-2;
		  d=0;
		}
	}

	cout << index << " " << index+1 << " " << index+2 << endl;
}
```