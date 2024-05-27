 对字符串进行 hash 的时候，可以将字符串视为一个 $p$ 进制的数。例如字符串 $abcde$，其对应的 hash 值为 $a \times p^4 + b \times p ^ 3 + c \times p^2 + d \times p + e$。

在本题中，我们需处理二位字符数组 $ch[N][N]$ 的 hash 值。首先，预处理左上角 $(1, 1)$ 到右下角 $(x, y)$ 的 hash 值。

设 $a[i][j]$ 表示同一行的 $ch[i][1]$ 到 $ch[i][j]$ 构成的字符串的 hash 值，我们用 $a[i][j] = a[i - 1][j] \times p + ch[i][j]$ 计算 hash 值。

设 $num[i][j]$ 表示 $(1, 1)$ 到 $(i, j)$ 的 hash 值，那么，通过 $num[i][j] = num[i - 1][j] \times p ^ n + a[i][j]$  计算 $(1, 1)$ 到 $(i, j)$ 的 hash 值。

利用这种方式求 hash 值，那么我们可以用类似二位前缀和的方式表示出每个长为 $a$，宽为 $b$ 的，右下角为 $(i, j)$ 的子矩阵的 hash 值。计算式子如下：

$num[i][j] - num[i - a][j] \times p^{an} - num[i][j - b] \times p^b + num[i - a][j - b] \times p^{a  n + b}$

###  参考代码

````c++
#include <bits/stdc++.h>
using namespace std;

const int N = 1000 + 5;
using ull = unsigned long long;
char mat[N][N], qmat[N][N];
int m, n, a, b, q;
int p = 133331;
ull base[N * N], num[N][N], qnum[N][N];
set<ull> s;

int main() {
  cin >> m >> n >> a >> b;
  base[0] = 1;
  for (int i = 1; i < m * n; i++) base[i] = base[i - 1] * p;
  for (int i = 1; i <= m; i++) cin >> mat[i] + 1;
  for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
      num[i][j] = num[i][j - 1] * p + mat[i][j] - '0';
    }
  }

  for (int i = 1; i <= m; i++) 
    for (int j = 1; j <= n; j++) {
      num[i][j] += num[i - 1][j] * base[n];
    }

  for (int i = a; i <= m; i++) {
    for (int j = b; j <= n; j++) {
      ull tmp = num[i][j] - num[i - a][j] * base[a * n] - num[i][j - b] * base[b] + num[i - a][j - b] * base[a * n + b];
      s.insert(tmp);
    }
  }

  cin >> q;
  while (q--) {
    for (int i = 1; i <= a; i++) {
      cin >> qmat[i] + 1;
    }
    ull tmp = 0;
    for (int i = 1; i <= a; i++) {
      for (int j = 1; j <= b; j++)
        qnum[i][j] = qnum[i][j - 1] * p + qmat[i][j] - '0';
    }
    for (int i = 1; i <= a; i++) 
      for (int j = 1; j <= b;j++) {
          qnum[i][j] += qnum[i - 1][j] * base[n];
        }
    
    if (s.find(qnum[a][b]) != s.end()) puts("1");
    else puts("0");
  }
}
````

