---
title: markdown 教程
updated: 2022-09-22 00:50:35Z
created: 2019-07-04 05:57:36Z
---




[toc]

####0.自动生成目录
[TOC]  (三个字母应全为大写或者全小写)
####1.标题设置

* 第一种方法：在标题文字前加#，一级标题为#，二级标题为##，三级标题为###，以此类推，最多六级，其中一级标题文字最大。
<br>
* 在标题文字下行输入--（符号前不可以有空格），可表示二级标题。在标题文字下行输入==，可表示一级标题。

####2.引用块注释
\>在一段文字前表示引用

示例：
>这是一段引用
>
>这是第二段
<br>
####3.文字效果
* 斜体     \*文字*     *文字*   
<br>
* 加粗     \*\*文字**  **文字**
  \* 可用 _ 代替
<br>
* 粗斜体 \*\*\*文字\*\*\*  ***文字***
<br>
* 文字底纹 \`内容\`  `内容`
<br>
* 文字下面加一行下划线 
新起一行输入___或***
___


####4.无序列表
* 无序列表
文字前添加 * 或 + 或 - 和一个空格
* 有序列表
罗马数字和英文句号和一个空格


####5.超链接

方式1:  [文字][1]
方式2: <www.google.com>




####6.插入图片

![](../../_resources/1-5.jpg)

####7.插入表格

|文字|文字|文字|
- | :-: | :-: | :-: | -:
|文字|文字|文字|

<br>
####8.插入代码(黑色底纹)

* 插入行内代码，即插入一个单词或者一句代码的情况，使用 `code` 这样的形式插入。
* 插入多行代码，用的最多的是，用六个 ` 包裹多行代码。当然也可以使用缩进的形式。


第一种: `test()`

第二种:简单文字出现一个代码框.使用     \```代码区```
例如: 
```c++
#include <iostream>
using namespace std;
int main()
{
    cout << "Hello, World!";
    return 0;
}
```

####9.插入脚注
文字内容[^1]




####10.复选框
在无序列表符号后面加上[ ]或者[x]代表选中或者未选中情况
- [ ] Markdown  
- [ ] JavaScript
- [ ] nice


####11.LaTeX公式
* 可以创建行内公式，例如$\Gamma(n)=(n-1)!\quad\forall n\in\mathbb N$。
<br>
* 可以创建行间公式
$$x=\dfrac{-b \pm \sqrt{b^2-4ac}}{2a}$$

####12.插入流程图

```flow 
st=>start: 开始 
en=>end: 结束
op=>operation: 条件
cond=>condition: YES or No?

st->op->cond
cond(yes)->en
cond(no)->op
```

####13.添加颜色
写法: \color{#376956}{cyan-blue}
效果: $\color{#376956}{cyan-blue}$


写法: \color{red}{red}
效果: $\color{red}{red}$


写法: \color{green}{green}
效果: $\color{green}{green}$




####14.数学公式
下标  $A_1$

大括号
$$ f(x)=\left\{
\begin{aligned}
x & = & \cos(t) \\
y & = & \sin(t) \\
z & = & \frac xy
\end{aligned}
\right.
$$

大于等于 $\ge$
小于等于 $\le$


两个quad空格	a \qquad b	a \qquad b	两个m的宽度
quad空格	   a \quad b	a \quad b	一个m的宽度
大空格	a\ b	a\ b	1/3m宽度
中等空格	a\;b	a\;b	2/7m宽度
小空格	a\,b	a\,b	1/6m宽度
没有空格	ab	ab\,	 
紧贴	a\!b	a\!b	缩进1/6m宽度

####15.iPad
这次是在iPad上面利用mweb写作，我正在测试能不能保存文件到iCloud。
看起来没问题。

####16 输出时分页
<div STYLE="page-break-after: always;"></div>


[^1]:这里是脚注内容
















