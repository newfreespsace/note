---
title: C++ 技巧
updated: 2022-10-25 02:44:16Z
created: 2022-10-25 02:05:27Z
latitude: 30.57281600
longitude: 104.06680100
altitude: 0.0000
---

## C++ 11 支持
### 1. 通过一对 {} 将值分配给容器
有选手这样写
```c++
pair<int, int> p;
p = make_pair(3, 4);
```
实际上可以这样写
```c++
pair<int, int> p;
p = {3, 4};
```
更复杂的也可以
```c++
pair<int, pair<char, long long> > p;
p = {3, {'a', 8ll}};
```
#### vector
```c++
vector<int> v;
v = {1, 2, 5, 2};
for (auto i: v)
    cout << i << ' ';
cout << '\n';
// prints "1 2 5 2"
```
#### deque
```c++
deque<vector<pair<int, int>>> d;
d = {{{3, 4}, {5, 6}}, {{1, 2}, {3, 4}}};
for (auto i: d) {
    for (auto j: i)
        cout << j.first << ' ' << j.second << '\n';
    cout << "-\n";
}
// prints "3 4
//         5 6
//         -
//	   1 2
//	   3 4
//	   -"
```
##### set
```c++
set<int> s;
s = {4, 6, 2, 7, 4};
for (auto i: s)
    cout << i << ' ';
cout << '\n';
// prints "2 4 6 7"
```
#### list
```c++
list<int> l;
l = {5, 6, 9, 1};
for (auto i: l)
    cout << i << ' ';
cout << '\n';
// prints "5 6 9 1"
```
#### array
```c++
array<int, 4> a;
a = {5, 8, 9, 2};
for (auto i: a)
    cout << i << ' ';
cout << '\n';
// prints "5 8 9 2"
```
#### tuple
```c++
tuple<int, int, char> t;
t = {3, 4, 'f'};
cout << get<2>(t) << '\n';
```
注意不适用于 **stack** 和 **queue**。
### 宏中参数的名称
您可以使用“#”符号来获取传递给宏的参数的确切名称：
```c++
#define what_is(x) cerr << #x << " is " << x << endl;
// ...
int a_variable = 376;
what_is(a_variable);
// prints "a_variable is 376"
what_is(a_variable * 2 + 1)
// prints "a_variable * 2 + 1 is 753"
```
### 隐藏功能（不是真的隐藏但不经常使用）

####  __gcd(value1, value2)

你不需要为gcd函数编写欧几里得算法，从现在开始我们可以使用了。此函数返回两个数字的 gcd。

#### __builtin_ffs(x)

此函数返回 x 的最低位的 $1$ 的位置+1。
比如说因为 10 的二进制为 1010， __builtin_ffs(10)  返回 $2$，因为最低位的1在第1位，加上1以后返回 2。

#### __builtin_clz(x)
#### __builtin_ctz(x)
#### __builtin_popcount(x)

### 可变参数函数和宏

### 基于范围的 for 循环
```c++
set<int> s = {8, 2, 3, 1};
for (auto it: s)
    cout << it << ' ';
// prints "1 2 3 8"
```
#### 如果需要更改值
```c++
vector<int> v = {8, 2, 3, 1};
for (auto &it: v)
    it *= 2;
for (auto it: v)
    cout << it << ' ';
// prints "16 4 6 2"
```

### Lambdas 表达式
定义如下：
```c++
[capture list](parameters) -> return value { body }
```
例如
```c++
auto f = [] (int a, int b) -> int { return a + b; };
cout << f(1, 2); // prints "3"
```
例如
```c++
vector<int> v = {3, 1, 2, 1, 8};
sort(begin(v), end(v), [] (int a, int b) { return a > b; });
for (auto i: v) cout << i << ' '; // print "8 3 2 1 1 "
```
### 原始字符串
可以在代码里嵌入一段原始字符串，该原始字符串不作任何转义。
```c++
string str1 = "a\nb\nc\n";
cout << str1 <<endl;
cout << "---------------------" <<endl;

string raw_str = R"(a\nb\nc\n)";
cout << raw_str <<endl;
cout << "---------------------" <<endl;

string raw_str2 = R"(x
y
z)";
cout << raw_str2 << endl;


```
输出结果为
```plain
a
b
c

---------------------
a\nb\nc\n
---------------------
x
y
z
```
###  merge 
```c++
merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);
//容器元素合并，并存储到另一个容器中
//注意：两个容器必须是有序的

//beg1 容器1开始迭代器
//end1 容器1结束迭代器
//beg2 容器2开始迭代器
//end2 容器2结束迭代器
//dest 目标容器开始迭代器
```
### 正则表达式

## C++14 支持
### 数字
现在，您可以使用单引号 （ ' ） 分隔数字的数字，以提高可读性
```c++
auto num = 10'000'000; // 整数， num = 10^7
auto fnum = 0.000'015'3; // 浮点数， fnum = 1.53 * 10^-5
auto bnum = 0b0100'1100'0110; // 二进制数，bnum = (010011000110)2 = (4C6)16 = 1222
```