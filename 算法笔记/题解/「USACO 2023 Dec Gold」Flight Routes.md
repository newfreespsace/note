## 题意

给出 $n$ 个节点的有向图，其中边只可能从编号小的节点指向编号大的节点。

现在给出任意两个节点间的路径数量的奇偶性（不存在路径即为 $0$ 条路径，算偶数），求该图存在多少条边。

输入的第一行包含整数 $N$。

接下来 $N-1$ 行，第 $i$ 行包含 $N-i$ 个整数。第 $i$ 行的第 $j$ 个整数表示从城市 $i$ 到城市 $i+j$ 的航线数目的奇偶性。

## 题解

这里，我们从样例2 出发，寻找题目的解法。

```plain
5
1111
101
01
1
```

该数据给出的关系如下：

1 -> 2 有奇数条路径，1 -> 3 有奇数条路径，1 -> 4 有奇数条路径，1 -> 5 有奇数条路径，

2 -> 3 有奇数条路径，2 -> 4 有偶数条路径，2 -> 5 有奇数条路径

3 -> 4 有偶数条路径，3 -> 5 有奇数条路径，

4 -> 5 有奇数条路径，

首先，我们考虑编号相邻的节点之间是否存在边。

 1 -> 2 有奇数条路径，又不可能通过其他节点从 $1$ 连接到 $2$​，所以存在一条 1->2 的边。

同理，

2 -> 3 有奇数条路径，所以存在边 2->3,

3 -> 4 有偶数条路径，所以 3 4 之间不存在边，

4 -> 5 有奇数条路径，所以存在边 3->4。

这是图现在的样子：

![image-20240129200609531](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20240129200609531.png)

接下来，我们考虑间隔 $1$ 的节点之间是否存在边，

1、3 之间有奇数条路径，由上图可知，1，3之间已经存在一条路径，所以1，3之间没有直接相连的边。

2、4 之间有偶数条路径，由上图可知，2，4之间不存在路径，所以2，4之间有没有直接相连的边。

3、5 之间有奇数条路径，由上图可知，3，5之间不存在路径，所以3，5之间有一条直接相连的边。

这是图现在的样子

![image-20240129214822674](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20240129214822674.png)

然后，我们考虑间隔2的节点之间是否存在边

1、4 之间有奇数条路径，由上图可知，1，4之间不存在路径，所以1，4之间需要连一条边。

2、5 之间有奇数条路径，由上图可知，2，5之间已有 $1$ 条路径，所以2，5之间不再需要连边。

这是图现在的样子

![image-20240129215007101](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20240129215007101.png)

最后，

1、5 之间有奇数条路径，由上图可知，2，5之间已有 $2$​ 条路径，所以1，5之间存在直接相连的边。下面是图最终的样子。

![image-20240129215123087](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20240129215123087.png)

从上面的分析，我们便可以得到本题的解法。按照上面的顺序，我们按间隔从小到大进行枚举，若 $i, j$ 之间的路径数与给出的关系不符，则在 $i、j$ 之间增加一条边，否则说明 $i、j $​ 之间不存在边。

这里，我们用 $f(x,y)$ 表示当前 $x$ 到 $y$ 的路径数，当增加一条边 $i, j$ 以后，$x$ 到 $y$ 增加了路径 $x$ 到 $i$ ，再从 $i$ 到 $j$，再从 $j$  到  $y$ 。所以 $f(x,y) += f(x,i) \times f(j, y)$，本题只关心 $f(x, y)$ 的奇偶，可用位运算代替。

