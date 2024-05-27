$n$ 个节点得到的完全图有 $\displaystyle \frac {n\times (n - 1)}2$，每条边可出现可不出现，故 $n$ 个节点可能得到的无向图有 $2^{\displaystyle \frac {n\times (n - 1)}2}$ 张。

现在我们从中去掉不连通的图。剩下的即为答案。

现在我们假设节点 $1$ 所在连通块的节点数量为 $k$，$k$ 个节点组成的连通图数量记为 $f[k]$。剩余 $n - k$ 个节点，得到的无向图有 $2^{\displaystyle \frac {(n - k)\times (n - k - 1)}2}$ 个。另外，是哪些节点与节点 $1$ 在同一个连通块，一共有 $C_{n - 1}^{k - 1}$ 种情况。

所以，节点 $1$ 所在连通块的节点数量恰好有 $k$ 个，这样的无向图一共有 $\displaystyle C_{n - 1}^{k - 1} \times f[k] \times 2^{ \frac {(n - k)\times (n - k - 1)}2}$ 张。现在，我们只需枚举枚举 $k$ 的值，就可以求出答案。

最后，还剩下最后一个问题，如何求 $f[k]$，这是与原问题仅仅规模不同的子问题，我们只需按照 $k$ 从小到大的顺序进行求解即可。



#### 初始条件

$f[1] = 1$

#### 转移方程

$\displaystyle  f[i] = 2^ {\frac {i\times (i - 1)}2} - \sum \displaystyle C_{i - 1}^{k - 1} \times f[k] \times 2^{ \frac {(i - k)\times (i - k - 1)}2}$， 其中 $1 \le k < i $

#### 目标

$f[n]$