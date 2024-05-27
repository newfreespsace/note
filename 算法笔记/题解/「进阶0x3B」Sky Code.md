考虑如何计算选出的 $4$ 个数公约数不为 $1$ 的方法数。

经过思考，我们发现：

从含有质因子 $2$ 的数中任选 $4$ 个均不互质，用集合 $A_2$ 表示这些数的集合

从含有质因子 $3$ 的数中任选 $4$ 个均不互质，用集合 $A_3$ 表示这些数的集合

从含有质因子 $5$ 的数中任选 $4$ 个均不互质，用集合 $A_5$ 表示这些数的集合

$\vdots$

但是，$A_2$ 和 $A_3$ 交集中的元素 $A_{2,3}$ 会重复计算，故我们需要在减去重复计算的地方，然后，集合 $A_{i,j,k}$ 中的元素对应的情况数会被减去 $3$ 次，而它们在开始只被计算了 $3$ 次，故我们需要再次将它们对答案的贡献加回来。依此类推。

对于本题，我们先预处理每个集合 $A_2、A_3、A_5、A_{2,3}、A_{2,5}、A_{2,3, 5}、\cdots$ 的大小，再利用容斥定理计算出问题的答案。

比如说整数 $2 \times 3^2 \times 5$，该数包含质因子 $2、3、5$，那么集合 $A_2、A_3、A_5、A_{2,3}、A_{25}、A_{35}、A_{2,3,5}$ 中均会包含该整数，但在具体实现的实现，我们不会关心集合包含哪些元素，我们只需关心集合中元素的数量。

## 参考代码

```c++

#include <bits/stdc++.h>
using namespace std;

const int N = 10000 + 5;
int n, A[N], num[N];

void divide(int x) {
    vector<int> prime;
    for (int i = 2; i * i <= x; i++) {
        if (x % i == 0) {
            prime.push_back(i);
            while (x % i == 0) x /= i;
        }
    }
    if (x > 1) prime.push_back(x);
    int sz = prime.size();
    for (int stat = 0; stat < (1 << sz); stat++) {
        int val = 1, cnt = 0;
        for (int bit = 0; bit < sz; bit++) {
            if ((stat >> bit) & 1) val *= prime[bit], cnt++;
        }
        A[val]++;
        num[val] = cnt;
    }
}

long long C4(int x) {
    if (x < 4) return 0;
    return 1ll * x * (x - 1) * (x - 2) * (x - 3) / 24;
}

void SkyCode() {
    memset(A, 0, sizeof A);
    memset(num, 0, sizeof num);
    for (int i = 1; i <= n; i++) {
        int x;
        cin >> x;
        divide(x);
    }

    long long ans = 0;
    for (int i = 2; i <= 10000; i++) {
        if (A[i] < 4) continue;
        if (num[i] & 1) ans += C4(A[i]);
        else ans -= C4(A[i]);
    }

    cout << C4(n) - ans << endl;
}

int main() {
    while (cin >> n) SkyCode();
}
```



