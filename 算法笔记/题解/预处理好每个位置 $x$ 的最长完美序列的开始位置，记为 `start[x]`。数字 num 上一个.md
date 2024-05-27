---
title: 预处理好每个位置 $x$ 的最长完美序列的开始位置，记为 `start[x]`。数字 num 上一个出现的位置保存在数组 `lst[num]`。
updated: 2023-09-18 08:16:38Z
created: 2023-09-18 07:20:15Z
latitude: 30.57281600
longitude: 104.06680100
altitude: 0.0000
---

预处理好每个位置 $x$ 的最长完美序列的开始位置，记为 `start[x]`。数字 num 上一个出现的位置保存在数组 `lst[num]`。


![屏幕截图 2023-09-18 152026.png](../_resources/屏幕截图%202023-09-18%20152026.png)

因为 $start[i - 1]$ 和  $i - 1$ 之间的数字均不相同，且 $start[i - 1]$ 前面的数字会和 $[start[i-1], i - 1]$之间的某个数字相同，所以 $start[i]$ 不小于 $start[i - 1]$。又 $start[i] \ge lst[A[i]] + 1$。记 $x = max\{ start[i - 1], lst[A[i]] + 1\}$，所以 $start[i] \ge x$。同时区间 $[x, i]$ 的元素均不相同，所以 $start[i] = x$。

下面是 start 数组的计算方法，其中lst 数组的初始值为 0。数组从 1 开始编号。
```c++
start[i] = max{ start[i - 1], lst[A[i]] + 1};
lst[A[i]] = i;
```

对于每个询问的区间 $[l, r]$，我们可以依次枚举每个位置 $x$，从而更新最长的完美序列长度。值得`特别注意`的是，`start[x]` 的值可能小于 $l$，此时最长完美序列的长度为 $x - l + 1$，其他情况长度为 $x - start[x] + 1$。每次询问的时间复杂度为 $O(n)$。

如何更快的计算出答案呢？本题寻找的是区间每个位置的最长完美序列的最大值，本来说区间最大值可以通过 ST 表维护，但本题询问的区间中有一部分位置，其对应的区间内最长完美序列长度并不是我们预处理的长度，因为这部分位置对应的 `start` 值小于询问区间左端点。幸运的是，`start` 数组具有单调性，我们可以直接二分查找出最后一个满足 $start[x] \ge l$ 位置 `pos`，该位置将区间分为了两部分，左边部分能够得到的最长完美序列即为 $pos - l$。而右边部分可通过 ST 表在 $O(1)$ 的时间计算出答案。每次询问，可以 $O(\log n)$ 的时间给出答案。
 

![屏幕截图 2023-09-18 160245.png](../_resources/屏幕截图%202023-09-18%20160245.png)


```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;
int n, m;
int A[N], start[N], B[N];
int lst[N * 10];
int f[N][20];

int query(int l, int r) {
    int ans = 0;
    int pos = lower_bound(start + l, start + r + 1, l) - start;
    ans = pos - l;
    
    if (pos <= r) {
        int t = log(r - pos + 1) / log(2);
        ans = max(ans, max(f[pos][t], f[r - (1 << t) + 1][t]));
    }

    return ans;
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);
    cin >> n >> m;
    for (int i = 1; i <= n; i++) cin >> A[i], A[i] += 1e6;
    for (int i = 1; i <= n; i++) {
        start[i] = max(start[i - 1], lst[A[i]] + 1);
        f[i][0] = i - start[i] + 1;
        lst[A[i]] = i;
    }

    for (int j = 1; j <= 19; j++) {
        for (int i = 1; i + (1 << j) - 1 <= n; i++) {
            f[i][j] = max(f[i][j - 1], f[i + (1 << (j - 1))][j - 1]);
        }
    }

    for (int i = 1; i <= m; i++) {
        int l, r;
        cin >> l >> r;
        l++, r++;
        cout << query(l, r) << "\n";
    }
}
```