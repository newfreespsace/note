![屏幕截图 2023-10-17 100413](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\屏幕截图 2023-10-17 100413.png)

考虑暴力做法，我们枚举右端点 $i$，再从左至右枚举左端点 $j$，检查区间最大值最小值是否满足条件，如果满足条件的话，那么此时的 $j$ 便是右端点为 $i$ 的时候，左端点的最小值。而如果不满足条件的话，$j$ 需要继续向右移动。但是，$j$ 需要一步一步的移动吗？在图中所示的情况，如果 $maxV- minV > k$，那么 $j$ 到 $minV$ 这些位置都可以直接跳过，因为在这些位置，区间最大值、最小值都不会改变，$j$ 可以直接跳到 $minV$ 的下一个位置。

故我们需要建两个单调队列，一个队列用来存储到目前位置 $i$ 为止所有可能成为区间最大值的位置，另一个存可能的区间最小值的位置。 $j$ 用来存储上一个位置 $i - 1$ 对应的满足条件的区间最左边端点。如果此时 $maxV - minV \le k$，则利用 $i - j + 1$ 更新答案，否则，$maxV$ 和 $minV$ 中更靠前的元素出队，$j$  更新为该元素的下一个位置。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 3e6 + 10;
int k, n, A[N];

int main() {
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> k >> n;
    for  (int i = 1; i <= n; i++) cin >> A[i];

    deque<int> minQ, maxQ;
    int j = 1;
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        while (minQ.size() && A[minQ.back()] > A[i]) minQ.pop_back();
        while (maxQ.size() && A[maxQ.back()] < A[i]) maxQ.pop_back();
        minQ.push_back(i);
        maxQ.push_back(i);

        while (minQ.size() && maxQ.size() && A[maxQ.front()] - A[minQ.front()] > k) {
            if (maxQ.front() < minQ.front()) {
                j = maxQ.front() + 1;
                maxQ.pop_front();
            } else if (maxQ.front() > minQ.front()) {
                j = minQ.front() + 1;
                minQ.pop_front();
            } else {
                j = minQ.front() + 1;
                minQ.pop_front();
                maxQ.pop_front();
            }
        }
        ans = max(ans, i - j + 1);
    }
    cout << ans << endl;
}
```

