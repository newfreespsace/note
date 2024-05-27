依次枚举每一枚导弹，该导弹要么用已经存在的系统进行拦截，要么新增一套系统进行拦截。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 50 + 5;
int n, A[N];
int dep = 0;

vector<int> MDS[N];

bool check(int k, int i) {
  if (MDS[i].size() == 0) return true;
  if (MDS[i].size() == 1) return A[k] != MDS[i][0];
  return (MDS[i][1] - MDS[i][0]) * (A[k] - MDS[i].back()) > 0;
}

bool dfs(int k) {
  if (k == n + 1) return true;
  for (int i = 1; i <= dep; i++) {
    if (!check(k, i)) continue;
    MDS[i].push_back(A[k]);
    if (dfs(k + 1)) return true;
    MDS[i].pop_back();
  }
  return false;
}

void Missile_Defence_System() {
  for (int i = 1; i <= n; i++) cin >> A[i];
  dep = 1;
  while (!dfs(1)) dep++;
  cout << dep << endl;
  for (int i = 1; i <= dep; i++) MDS[i].clear();  
}

int main() {
  while (cin >> n && n) Missile_Defence_System();
}
```

借助 *导弹拦截* 的贪心思路，我们应该在能够拦截当前导弹的系统中选择拦截高度最低的那套系统进行拦截，这样总共使用的系统数量最少，但是本题存在着上升和下降两套系统，我们需分情况讨论即可。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 50 + 5;
int n, A[N];
int dep = 0;

vector<int> up, down;

bool dfs(int k) {
  if (up.size() + down.size() > dep) return false;
  if (k == n + 1) return true;

  int t = upper_bound(up.begin(), up.end(), A[k], greater<int>()) - up.begin();
  if (t == up.size()) {
    up.push_back(A[k]);
    if (dfs(k + 1)) return true;
    up.pop_back();
  } else {
    int num = up[t];
    up[t] = A[k];
    if (dfs(k + 1)) return true;
    up[t] = num;
  }

  t = upper_bound(down.begin(), down.end(), A[k]) - down.begin();
  if (t == down.size()) {
    down.push_back(A[k]);
    if (dfs(k + 1)) return true;
    down.pop_back();
  } else {
    int num = down[t];
    down[t] = A[k];
    if (dfs(k + 1)) return true;
    down[t] = num;
  }

  return false;
}

void Missile_Defence_System() {
  for (int i = 1; i <= n; i++) cin >> A[i];
  up.clear();
  down.clear();
  dep = 1;
  while (!dfs(1)) dep++;
  cout << dep << endl;
}

int main() {
  // freopen("1.in", "r", stdin);
  while (cin >> n && n) Missile_Defence_System();
}
```

