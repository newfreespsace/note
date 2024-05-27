---
title: STL
updated: 2022-08-09 06:26:07Z
created: 2019-10-15 13:09:42Z
---

## STL
STL 是“Standard Template Library”的缩写，中文译为“标准模板库”。
STL 是 C++ 标准库的一部分，不用单独安装。
C++ 对模板（Template）支持得很好，STL 就是借助模板把常用的数据结构及其算法都实现了一遍，并且做到了数据结构和算法的分离。

### 栈

```c++
#include <stack>
usng namespace std;

stack <int> s;  // 栈s中存放的数据类型为int
```
* top()
    返回栈顶元素。如果栈为空，返回值未定义
* push(int x)：
    元素x入栈
* pop()
    弹出栈顶元素，注意无返回值
* size()   
    返回栈中元素的个数。
* empty()
    在栈中没有元素的情况下返回 true。

