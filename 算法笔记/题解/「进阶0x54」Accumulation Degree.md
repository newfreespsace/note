![graph](C:\Users\Lee\OneDrive\笔记\算法学习\assets\graph.png)

首先，我们考虑暴力做法，枚举每一个节点，计算将该节点作为源点时的最大流量。如图所示，我们计算将 $1$ 作为源点时的最大流量，自然，每个河道能够通过的最大流量取决于河道本身的容量和其连接的子节点能够输送的流量中更小的那一个。

用 $D[x]$ 表示以 $x$ 为根的子树中，$x$ 作为源点能够流向子树的最大流量。

$ D[x] = \sum \min \{D[y], c(x, y)\} $

上面的式子中 $y$ 表示 $x$ 的子节点，$c(x, y)$ 表示 $x$ 和 $y$ 之间的河道。当 $x$ 不存在子节点的时候，我们认为 $D[x] = 0$。

在实现的实现，我们通过节点的度数判断子节点是否为叶节点。

```c++
void dp(int x) {
    v[x] = 1;
    D[x] = 0;
    for (auto [y, z] : e[x]) {
        if (v[y]) continue;
        dp(y);
        if (deg[y] == 1) D[x] += z;
        else D[x] += min(D[y], z);
    }
}

void AccumulationDegree() {
    cin >> n;
    for (int i = 1; i <= n; i++) e[i].clear();
    for (int i = 1; i <= n; i++) deg[i] = 0;
    for (int i = 1; i < n; i++) {
        int x, y, z;
        cin >> x >> y >> z;
        e[x].push_back({y, z});
        e[y].push_back({x, z});
        deg[x]++, deg[y]++;
    }
    int ans = 0;
    for (int i = 1; i <= n; i++) {
        memset(v, 0, sizeof v);
        dp(i);
        ans = max(ans, D[i]);
    }
    cout << ans << endl;
}
```

因为将每个点作为根节点进行了一次深搜，时间复杂度为 $O(N^2)$。

下面的 **二次扫描与换根法** 可在 $O(N)$ 的时间求出每个节点作为源点的最大流量。

首先固定 `root` 为根，先进行一次上面的深搜，求出了 $D$​ 数组。

![graph (1)](C:\Users\Lee\OneDrive\笔记\算法学习\assets\graph (1).png)

我们用 $F[x]$ 表示把 $x$ 作为源点的最大流量值。现在我们从 root 开始，重新进行一次搜索，首先 $F[root]$ 的值等于 $D[root]$。

现假设 $F[x]$ 已经求出，考虑如何求出其子节点 $y$ 的 $F$ 值。

首先，$y$ 可以流向它的子树的最大流量为 $D[y]$。

接下来，我们考虑 $y$ 能够流向其父节点 $x$ 的最大流量，因为 $x$ 作为源点的最大流量为 $F[x]$，所以 $x$ 流向除 $y$ 以外的其余子节点的最大流量为 $F[x] - \min\{D[y], c(x,y)\}$。所以 $y$ 能够流向 $x$ 的最大流量为 $\min \{ F[x] - \min\{D[y], c(x,y)\}, c(x, y) \}$。

由此，我们得到公式 $F[y] = D[y] + \min \{ F[x] - \min\{D[y], c(x,y)\}, c(x, y) \}$。

注意到特殊情况：

* 当 $deg[y]=1, deg[x] > 1$ 时，此时 $D[y] = 0$, $y$ 流向 $x$ 的最大流量为 $\min \{c(x, y), F[x] - c(x, y) \}$​
* 当 $deg[x] = 1$ 时，y 流向 x 的最大流量即为 $c(x, y)$​。

### 完整参考代码

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 2e5 + 10;
const int inf = 0x3f3f3f3f;
vector<pair<int, int> > e[N];
int n, D[N], deg[N], F[N];
bool v[N];

void dp(int x) {
    v[x] = 1;
    D[x] = 0;
    for (auto [y, z] : e[x]) {
        if (v[y]) continue;
        dp(y);
        if (deg[y] == 1) D[x] += z;
        else D[x] += min(D[y], z);
        
    }
}

void dfs(int x) {
    v[x] = true;
    for (auto [y, z] : e[x]) {
        if (v[y]) continue;
        if (deg[x] == 1) F[y] = D[y] + z;
        else if (deg[y] == 1) F[y] = min(F[x] - z, z);
        else  F[y] = D[y] + min(F[x] - min(z, D[y]), z);
        dfs(y);
    }
}

void AccumulationDegree() {
    cin >> n;
    for (int i = 1; i <= n; i++) e[i].clear();
    for (int i = 1; i <= n; i++) deg[i] = 0;
    for (int i = 1; i < n; i++) {
        int x, y, z;
        cin >> x >> y >> z;
        e[x].push_back({y, z});
        e[y].push_back({x, z});
        deg[x]++, deg[y]++;
    }
    
    memset(v, 0, sizeof v);
    dp(1);
    
    memset(v, 0, sizeof v);
    F[1] = D[1];
    dfs(1);

    int ans = 0;
    for (int i = 1; i <= n; i++) ans = max(ans, F[i]);
    cout << ans << endl;
}

int main() {
    int t;
    cin >> t;
    while (t--) AccumulationDegree();
}
```



