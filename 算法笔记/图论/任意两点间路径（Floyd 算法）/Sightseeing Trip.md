

在 Floyd 算法中， 在外层循环 $k$ 刚开始时，$d(i, j)$ 保存 '经过编号不超过 $k - 1$ 的节点' 从 $i$ 到 $j$ 的最短路长度。

于是， $d(i, j) + a[j][k] + a[k][i]$ ($1 \le i < j < k$) 便表示了经过节点 $k$ ，且其它节点编号均小于 $k$ 的一个环的长度。枚举所有满足条件的 $i, j$，便可以得到

* 经过节点 $k$
* 其余节点编号均小于 $k$

的最小环长度。

对所有的 $k$ 值进行上面的计算，便可以得到整张图的最小环。

另外，本题需要输出环，当通过节点 $k$ 得到最小环的时候，已经可以得知环经过了 $i、k、j$ 。

![image-20240417093340726](C:\Users\Lee\OneDrive\笔记\算法笔记\图论\任意两点间路径（Floyd 算法）\assets\image-20240417093340726.png)

为了得到环，还需计算此时 $i$ 到 $j$ ，满足经过的节点数均小于 $k$ 的最短路径。为了得到该路径，我们使用数组 $pos[i][j]$ 记录从 $i$ 到 $j$ 的最短路中经过的最大节点编号。

![image-20240417095108171](C:\Users\Lee\OneDrive\笔记\算法笔记\图论\任意两点间路径（Floyd 算法）\assets\image-20240417095108171.png)

递归处理 $i$ 到 $pos(i, j)$ 和 $pos(i, j)$ 到 $j$ 的路径，便可以得到此时 $i$ 到 $j$ 的最短路经过的所有节点。