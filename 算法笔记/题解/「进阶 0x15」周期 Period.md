## 引理1

$S[1 \sim n]$ 具有长度为 $len < n$ 的循环节的充要条件是 $S[len + 1 \sim n] = S[1 \sim n - len]$。

> 我们认为这种情况 $\text{ABCDABCDABCDABC}$ 存在循环节 $\text{ABCD}$ ，如果不认可这种情况，需检查字符串的长度是否是循环节长度的倍数。

### 证明
![image-20231102094933055](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20231102094933055.png)

$S[1 \sim n]$ 具有长度为 $len < n$ 的循环节 $\iff$ $S[x] = S[x + len](1 \le x \le n - len)$  

即我们有 $S[1] = S[1 + len]$，$S[2] = S[2 + len]$，$S[3] = S[3 + len]$，$\cdots$，$S[n - len] = S[n]$。

所以原条件 $\iff S[1 \sim n - len] = S[len + 1 \sim n]$。

## 引理2

$S[1\sim n]$ 分别存在着长度为 $a$ 和 $b$ $(a > b)$的循环节，那么 S 同时存在着长度为 $a - b$ 的循环节，进一步，$S$ 存在着长度为 $a \% b $ 的循环节。

### 证明

$S[x] = S[x + a]$， $S[x]= S[x + b]$

所以 $S[x + a - b] = S[x + a]$, $S[x] = S[x + a]$。

所以 $S[x + a - b] = S[x]$

---

因为 $j = next[n]$ 是满足 $S[n - j + 1 \sim n] = A[1 \sim j]$ 的最大整数，所以 $S[1 \sim n - j]$ 便是最小循环节。对本题来说，需判断 $n - next[n]$ 是否小于 $n$ 且能整除 $n$ 。
