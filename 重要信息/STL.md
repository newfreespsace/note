---
title: STL
updated: 2022-08-09 06:26:05Z
created: 2019-10-31 14:03:23Z
---

multiset
>multiset<T> 容器就像 set<T> 容器，但它可以保存重复的元素。这意味我们总可以插入元素，当然必须是可接受的元素类型。默认用 less<T> 来比较元素，但也可以指定不同的比较函数。在元素等价时，它必须返回 false。

例如：
std::multiset<string, std::greater<string>> words{{"dog", "cat", "mouse"}, std::greater<string>()};

这条语句定义了一个以 string 为元素的 multiset，它以 greater<string> 为构造函数的第二个参数。构造函数的第一个参数是一个初始化列表，它为这个容器指定了三个初始元素。和 set 一样，如果它的两个元素相等，那么它们就是匹配的。在一个有比较运算符 comp 的 multiset 中，如果表达式 !(a comp b) && !(b comp a) 为 true，那么元素 a 和 b 就是相等的。
multiset 容器和 set 容器有相同的成员函数，但是因为 multiset 可以保存重复元素，有些函数的表现会有些不同。和 set 容器中的成员函数表现不同的是：

insert() 总是可以成功执行。当插入单个元素时，返回的迭代器指向插入的元素。当插入一段元素时，返回的迭代器指向插入的最后一个元素。

emplace() 和 emplace_hint() 总是成功。它们都指向创建的新元素。
find() 会返回和参数匹配的第一个元素的迭代器，如果都不匹配，则返回容器的结束迭代器。

equal_range() 返回一个包含迭代器的 pair 对象，它定义了一个和参数匹配的元素段。如果没有元素匹配的话，pair 的第一个成员是容器的结束迭代器；在这种情况下，第二个成员是比参数大的第一个元素，如果都没有的话，它也是容器的结束迭代器。

lower_bound() 返回和参数匹配的第一个元素的迭代器，如果没有匹配的元素，会返回容器的结束迭代器。返回的迭代器和 range() 返回的 pair 的第一个成员相同。

upper_bound() 返回的迭代器和 equal_range() 返回的 pair 的第二个成员相同。

count() 返回和参数匹配的元素的个数。