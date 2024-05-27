![image-20240516101039794](C:\Users\Lee\OneDrive\笔记\算法笔记\题解\assets\image-20240516101039794.png)

首先，我们将谷仓按如下规则映射为一个数字串：

起点对应为 0，每条边与其长度对应，顺时针方向，如果顶点处的内角为90度，则将该顶点与 $-1$ 对应，若内角为 $270$ 度，则与 $-2$ 对应。

如上图，映射为数字串 $0, 4, -1, 4, -2, 5, -1, 3, -1, 8, -1, 6, 0$。

接下来，考虑从一个顶点 $i$ 走到另一个顶点 $j$，假设能够判断出所处的位置，那么该段路程对应的数字串在原数字串的所有子串中只出现一次。

我们预处理所有的子串，统计每个子串出现的次数。接下来，再枚举贝西可能的移动路程，判断该路程是否只出现一次，若只出现一次，则可确定贝西所在的位置。

另外，需越处理从每个顶点出发的最小距离。由此计算旅行距离增加的值。

#### 参考代码

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 200 + 5;
pair<int, int> A[N];

int n;
int best[N];

int main() {
  cin >> n;
  for (int i = 0; i < n; i++) cin >> A[i].first >> A[i].second;
  vector<int> str;
  str.push_back(0);
  for (int i = 0; i < n; i++) {
    int j = (i + 1) % n, k = (i + 2) % n;
    int xx1 = A[j].first - A[i].first, yy1 = A[j].second - A[i].second;
    int xx2 = A[k].first - A[j].first, yy2 = A[k].second - A[j].second;
    str.push_back(abs(xx1) + abs(yy1));
    if (xx1 * yy2 - xx2 * yy1 > 0) str.push_back(-2);
    else str.push_back(-1);
  }
  str.back() = 0;

  multiset<vector<int> >S;
  for (int i = 1; i < n; i++) {
    for (int j = i; j <= n; j++) {
      S.insert(vector<int>(str.begin() + 2 * i, str.begin() + 2 * j + 1));
    }
  }
  
  for (int i = 1; i < n; i++) best[i] = best[i - 1] + str[2 * i - 1];
  best[n] = 0;
  for (int i = n - 1; i >= 0; i--) best[i] = min(best[i], best[i + 1] + str[2 * i + 1]);

  int ans = 0;
  for (int i = 1; i < n; i++) {
    int j = i;
    int cost = 0;
    for (; j <= n; j++) {
      if (S.count(vector<int>(str.begin() + 2 * i, str.begin() + 2 * j + 1)) == 1) break;
      cost += str[2 * j + 1];
    }
    ans = max(ans, cost + best[j] - best[i]);
  }
  cout <<  ans << endl;
}
```

