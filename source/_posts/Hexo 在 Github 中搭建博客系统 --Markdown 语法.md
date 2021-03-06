---
title: Hexo 在 Github 中搭建博客系统 --Markdown 语法
date: 2016-10-28 15:06:15
img: '/medias/3.jpg'
categories:
- Hexo
---

## Hexo 在 Github 中搭建博客系统 --Markdown 语法

前面我们已经知道怎么搭博客系统、发布、创建文章, 现在到了创建内容的时候, 需要熟练掌握 Markdown 的语法

### 基本语法

(0) 特殊字符
--------

之所以把特殊字符放到最前面, 就是因为 Markdown 对特殊字符处理不是很友好, 很容易出现很奇怪的异常, 其实就是因为文章中包含了特殊字符无法编译通过导致 Hexo 无法渲染  
在写博客时, 一定注意不要写这些字符, 如果要写, 就进行转义  
有些人可能说可以使用反斜杠 \\ 来处理, 我这里不建议, 因为我使用的时候没有解决问题。  
下面是比较常见的几个:

```bash
! ! — 惊叹号Exclamation mark 
” " " 双引号Quotation mark 
# # — 数字标志Number sign 
$ $ — 美元标志Dollar sign 
% % — 百分号Percent sign 
& & & Ampersand 
‘ ' — 单引号Apostrophe 
( ( — 小括号左边部分Left parenthesis 
) ) — 小括号右边部分Right parenthesis 
\* \* — 星号Asterisk 
+ + — 加号Plus sign 
< < < 小于号Less than 
= = — 等于符号Equals sign 
> > > 大于号Greater than 
? ? — 问号Question mark 
@ @ — Commercial at 
\[ \[ --- 中括号左边部分Left square bracket 
\\ \\ --- 反斜杠Reverse solidus (backslash) 
\] \] — 中括号右边部分Right square bracket 
{ { — 大括号左边部分Left curly brace 
| | — 竖线Vertical bar 
} } — 大括号右边部分Right curly brace 
```

**_特别注意的是小括号 () 大括号 {} , 如果不小心写了, 你执行 hexo s –debug 可能报一些莫名其妙的错误!_**  

[这里还有更多关于特殊字符如何转义的内容](http://www.cnblogs.com/xcsn/p/3559624.html)

(1) 标题
------

在文字前加上 #号

```bash
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题

大标题
=
小标题
-
```

预览效果：

# 一级标题

## 二级标题

### 三级标题

#### 四级标题

##### 五级标题

###### 六级标题

大标题
===

小标题
---

(2) 粗体斜体
--------

```bash
*斜体文本*    _斜体文本_
**粗体文本**    __粗体文本__
***粗斜体文本***    ___粗斜体文本___
```

预览效果：  
_斜体文本_
**粗体文本**
___粗斜体文本___

(3) 链接
------

```bash
常用链接方法
文字链接 [链接名称](https://chwshuang.github.io)
网址链接 <https://chwshuang.github.io>


高级链接技巧
这个链接用 1 作为网址变量 [Google][1].
这个链接用 yahoo 作为网址变量 [Yahoo!][yahoo].
然后在文档的结尾为变量赋值（网址）

  [1]: http://www.google.com/
  [yahoo]: http://www.yahoo.com/
```

预览效果：  
文字链接 [链接名称](http://hushuang.me)  
网址链接 [http://hushuang.me](http://hushuang.me)

高级链接技巧  
这个链接用 1 作为网址变量 [Google](http://www.google.com/).  
这个链接用 yahoo 作为网址变量 [Yahoo!](http://www.yahoo.com/).

(4) 列表
------

普通无序列表

```bash
- 列表文本前使用 [减号+空格]
+ 列表文本前使用 [加号+空格]
* 列表文本前使用 [星号+空格]
```

普通有序列表

```bash
1. 列表前使用 [数字+空格]
2. 我们会自动帮你添加数字
7. 不用担心数字不对，显示的时候我们会自动把这行的 7 纠正为 3
```

列表嵌套

```bash
1. 列出所有元素：
    - 无序列表元素 A
        1. 元素 A 的有序子列表
    - 前面加四个空格
2. 列表里的多段换行：
    前面必须加四个空格，
    这样换行，整体的格式不会乱
3. 列表里引用：

    > 前面空一行
    > 仍然需要在 >  前面加四个空格
```

(5) 引用
------

普通引用

```bash
> 引用文本前使用 [大于号+空格]
> 折行可以不加，新起一行都要加上哦
```

引用里嵌套引用

```bash
> 最外层引用
> > 多一个 > 嵌套一层引用
> > > 可以嵌套很多层
```

引用里嵌套列表

```bash
> - 这是引用里嵌套的一个列表
> - 还可以有子列表
>     \* 子列表需要从 - 之后延后四个空格开始
```

(6) 图片
------

跟链接的方法区别在于前面加了个感叹号!，这样是不是觉得好记多了呢?

```bash
![图片名称\](http: //图片地址)
当然，你也可以像网址那样对图片网址使用变量

这个链接用 1 作为网址变量 [ Google] [ 1].
然后在文档的结尾位变量赋值(网址)

[1]: http: //www.google.com/logo.png
也可以使用 HTML 的图片语法来自定义图片的宽高大小
```


(7) 换行
------

如果另起一行，只需在当前行结尾加 2 个空格

```bash
在当前行的结尾加 2 个空格  
这行就会新起一行
如果是要起一个新段落，只需要空出一行即可。
```

(8) 分隔符
-------

如果你有写分割线的习惯，可以新起一行输入三个减号 -。当前后都有段落时，请空出一行：

```bash
前面的段落

---

后面的段落
```

(9) 小型文本
--------

Markdown 语法：

```bash
<small>文本内容</small>
```

预览效果：  
我是正常文字  
<small>我是小型文字</small>

(10) 注释
-------

用 html 的注释，好像只有这样？

```bash
<!-- 注释 -->
```

(11) 表格
-------

Markdown 的扩展语法，hexo 已经支持

```bash
| 参数           | 说明                 |   默认值            |
| ------------- |:-------------------:|:------------------:|
| host          | 远程主机的地址         |                    |
| user          | 使用者名称            |                    |
| root          |  远程主机的根目录      |                    |
| port          | 端口                 |       22           |
| delete        | 删除远程主机上的旧文件   |  true              |
| verbose       | 显示调试信息           |   true             |
| ignore\_errors | 忽略错误              |     false          |
```

<table><thead><tr><th>参数</th><th align="center">说明</th><th align="center">默认值</th></tr></thead><tbody><tr><td>host</td><td align="center">远程主机的地址</td><td align="center"></td></tr><tr><td>user</td><td align="center">使用者名称</td><td align="center"></td></tr><tr><td>root</td><td align="center">远程主机的根目录</td><td align="center"></td></tr><tr><td>port</td><td align="center">端口</td><td align="center">22</td></tr><tr><td>delete</td><td align="center">删除远程主机上的旧文件</td><td align="center">true</td></tr><tr><td>verbose</td><td align="center">显示调试信息</td><td align="center">true</td></tr><tr><td>ignore_errors</td><td align="center">忽略错误</td><td align="center">false</td></tr></tbody></table>

最后:  
还是要注意特殊字符的处理, 其他的就是排版样式的问题, 特殊字符没处理好, 就是 Hexo 渲染报错


原文地址 \[blog.csdn.net\](https://blog.csdn.net/chwshuang/article/details/52350551)