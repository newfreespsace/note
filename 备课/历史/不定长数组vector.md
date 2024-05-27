---
title: 不定长数组vector
updated: 2022-08-09 06:26:09Z
created: 2019-11-22 06:25:03Z
---

### vector

头文件
```c++
#include <vector>

```
---
创建：
```c++
vector<int> vec
```
---
尾部插入元素：
```c++
vec.push_back(a)
```
---
使用下标访问元素：
```c++
vec[0],vec[1],vec[2]....
```
---
使用迭代器访问元素
```c++
vector<int>::iterator it;
for( it=vec.begin(); it != vec.end(); ++it )
    cout << *it << endl;
````
---
插入元素
```c++    
vec.insert( vec.begin() + i, a );
```
---

删除元素
```c++
vec.erase( vec.begin() + 2 ); //删除第3个元素

vec.erase( vec.begin() + i, vec.end() + j ); // 删除区间[i,j-1];区间从0开始
```
---

向量大小
```c++
vec.size()
```
---
清空
```c++
vec.clear()
```


