---
title: HILO 题解
updated: 2022-08-15 09:06:01Z
created: 2022-08-15 01:17:33Z
latitude: 29.72754100
longitude: 113.61577500
altitude: 0.0000
---

HILO 题解

$O(n^2)$ 暴力算法
```c++
#include <bits/stdc++.h>
using namespace std;
int n, p[200010];

int main() {
	cin >> n;
	for (int i = 1; i <= n; i++) cin >> p[i];
	
	for (int x = 0; x <= n; x++) {
		// mn ~ mx 为当前我们能确定的猜测的数所在的区间（mn, mx）
		int mn = 0, mx = n + 1, ans = 0;
		bool hi = false;
		for (int i = 1; i <= n; i++) {
			if (p[i] > mx || p[i] < mn) continue;
			if (p[i] > x) {
				hi = true;
				mx = p[i];
			} else {
				ans += hi;
				hi = false;
				mn = p[i];
			}
		}
		cout << ans << endl;
	}
}
```

另一个解决方案在最坏的情况下是 $O(n^2)$ 的时间复杂度，我们为每个 $x$ 跟踪每个 ``HI`` 和 ``LO`` 出现的位置，通过这两个列表找出 ``HILO`` 的数量。但是，这个解决方案只能通过均匀的随机数据，因为在这种条件下，对每个 $x$ 来说，期望的 ``HI`` 和 ``LO`` 的数量为 $O(\log n)$，这样，总的时间复杂度为 $O(n\log n)$。

为了保持跟踪这两个列表，我们通过使用两个单调栈，本质上，我们维护了一个用来保存 Bessie 说  ``LO`` 的位置，并且排过序的索引列表。当我们检查的值从 $x$ 过渡为 $x+1$ 时，我们知道最后一个 Bessie 会说``LO`` 的位置为值为 $x+1$ 的位置。如果 Bessie 在 $x + 1$ 时没有对某个 $k$ 说 ``LO``，那么她在所有的 $y > x + 1$ 的时候也不会对某个 $k$ 说 ``LO``，所以，我们可以删除排列中所有 $x + 1$ 之后说 ``LO`` 的位置。
```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;
int n, pos[N];
vector<int> los[N], his[N];

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        pos[x] = i;
    }

    for (int i = 1; i <= n; i++) {
        los[i] = los[i - 1];
        while (!los[i].empty() && los[i].back() > pos[i]) los[i].pop_back();
        los[i].push_back(pos[i]);
    }

    for (int i = n - 1; i >= 0; i--) {
        his[i] = his[i + 1];
        while (!his[i].empty() && his[i].back() > pos[i + 1]) his[i].pop_back();
        his[i].push_back(pos[i + 1]);
    }

    for (int i = 0; i <= n; i++) {
        int ans = 0;

        int j = 0;
        for (int x = 0; x < los[i].size(); x++) {
            if (j < his[i].size() && his[i][j] < los[i][x]) ans++;
            while (j < his[i].size() && his[i][j] < los[i][x]) j++;
        }  
        
        cout << ans << endl;
    }
}
```

断言1：假设 $p$ 为给定的排列。如果索引 $i$ 是一个 ``HILO`` 对中 ``LO`` 的位置，那么该对中的 ``HI`` 的对应的索引 $k$ 满足条件 $k < i$, $p[k] > p[i]$，并且 $p[k]$ 是最小的。

证明：
如果不存在这样的 $k$，那么在 $i$ 之前就没有更大的元素，所以 $i$ 不可能是配对 ``HILO`` 中的 ``LO``。
而如果在 $p[k]$ 处 Bessie 说的是 ``LO``，那么 Elsie 就会跳过 $i$ 处，所以 $i$ 不可能是配对 ``HILO`` 中的 ``LO``。
因此，Bessie 最后一次在 $p[i]$ 之前说 ``HI`` 一定是在 $k$ 处。

知道了这一点以后，我们可以使用单调栈计算一个数组 ``prv``，其中$prv[i]=k$，含义如上所述。

断言2：令索引 $j$ 表示 Bessie 在对 $p[i]$ 说 ``LO`` 之前的最后一个 ``LO``，对于每一个没有跳过 $p[i]$ 的 $x$ 来说，$j$ 都是相等的。

证明：令 $j_1 < j_2 < i$ 并且 $p[j_1], p[j_2] < p[i]$。如果 Bessie 对 $p[i]$ 说了 ``LO``，那么有 $3$ 种可能：
1. Bessie 从未对 $p[j_1]$ 或 $p[j_2]$ 说过 ``LO``，因为它已经在某个索引 $k < j_1$，$p[k]>p[j_1]、p[j_2]$ 时说过 ``LO``。
2. Bessie 总是在 $p[j_1]$ 说 ``LO``，但从不对 $p[j_2]$ 说 ``LO``，因为她在某个索引 $j_1 \le j < j_2$ 处说过 ``LO``，并且 $p[k]>p[j_2]$。
3. Bessie 总是对 $p[j_1]$ 和 $p[j_2]$ 都说了 ``LO``，因为上述情况都没有发生。

如果过渡使得 Bessie 停止对 $i$ 之前的某个索引说 ``LO``，那么它一定会导致它停止对 $p[i]$ 说 ``LO``。因此，$j$ 对每个 $p[i]$ 都是一样的。

断言3：$j$ 的定义如上。索引 $i$ 是 ``HILO`` 对中的 ``LO``，当且仅当 $prv[i]$ 存在，且 $j$ 不存在或 $prv[i] \neq prv[j]$。

证明：如果 $prv[i]$ 不存在，那么 Bessie 在对 $p[i]$ 说 ``LO`` 之前从未说过 ``HI`` ，所以它不可能是 ``HILO`` 配对中的 ``LO``。另外，如果$prv[i]≠prv[j]$，那么 Bessie 在对 $p[i]$ 说 ``LO`` 之前说的最后一个 ``HI``，根据定义一定是在对 $p[j]$ 说  ``LO`` 之后。在这种情况下，索引i是 ``HILO`` 对中的 ``LO``。

知道了这一点，我们就可以为每个索引确定它是否是  ``HILO`` 对中的 ``LO``，从而确定每个 $X$ 的 ``HILO`` 对有多少个。

```c++
#include <iostream>
#include <stack>

int main() {
    std::cin.tie(0)->sync_with_stdio(0);
    int n, pos[200001], prv[200001];
    bool hilo[200001];
    std::cin >> n;
    for (int i = 1; i <= n; i++) {
        int p;
        std::cin >> p;
        pos[p] = i;
    }

    std::stack<int> stck;
    stck.push(0);
    for (int i = n; i > 0; i--) {
        while (stck.top() > pos[i]) stck.pop();
        prv[pos[i]] = stck.top();
        stck.push(pos[i]);
    }
    while (stck.size() != 1) stck.pop();

    std::cout << "0\n";
    int cnt = 0;
    for (int i = 1; i <= n; i++) {
        while (stck.top() > pos[i]) {
            cnt -= hilo[stck.top()];
            stck.pop();
        }
        hilo[pos[i]] = prv[pos[i]] != 0 &&
                       (stck.top() == 0 || prv[pos[i]] != prv[stck.top()]);
        stck.push(pos[i]);
        cnt += hilo[pos[i]];
        std::cout << cnt << '\n';
    }
    return 0;
}
```






