## 子任务1

在 $w = 1$ 的时候，我们可以认为两头奶牛相遇时，并没有发生碰撞，而是它们直接穿过对方，不受任何影响的到达左右端点，故我们可以将奶牛按照到达时间排序，然后依次枚举每头奶牛，从而计算出时间 $T$。最后，我们再枚举每对奶牛，看它们能否在时间 $T$ 内相遇即可。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 5e4 + 10;
struct node {
    int w, x, d;
};
node A[N];
int n, l;

int main() {
    cin >> n >> l;
    int sum = 0;
    for (int i = 1; i <= n; i++) cin >> A[i].w >> A[i].x >> A[i].d;
    sort(A + 1, A + n + 1, [](const node &a, const node &b) {
        if (a.d == -1 && b.d == -1) return a.x < b.x;
        if (a.d == -1 && b.d == 1) return a.x < l - b.x;
        if (a.d == 1 && b.d == -1) return l - a.x < b.x;
        return l - a.x < l - b.x;
    });

    int t = A[(n + 1) / 2].d == 1 ? l - A[(n + 1) / 2].x : A[(n + 1) / 2].x;

    int cnt = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = i + 1; j <= n; j++) {
            if (A[i].d == 1 && A[j].d == -1 && A[i].x <= A[j].x && A[i].x + 2 * t >= A[j].x) cnt++;
            if (A[i].d == -1 && A[j].d == 1 && A[j].x <= A[i].x && A[j].x + 2 * t >= A[i].x) cnt++;
        }
    }
    cout << cnt << endl;
}
```

## 子任务2

这里没有了 $w = i$ 的限制，时间 $T$ 不再等于 $A[(n + 1) / 2]$，不过我们依然可以认为奶牛相遇时可以传过对方，只是穿过后，重量将会变为对方的体重，现在我们有了一个新的问题，每头奶牛到达端点时其体重是多少呢？注意到奶牛实际上并不能穿过相遇奶牛，不论它们相遇后怎么改变方向，它们相对的顺序并不会改变。

比如说按照可以穿过相遇奶牛的假设，第 $i$ 头牛最先到达左边的端点，但实际上是最左边的奶牛最先到达左端点，第 $j$ 头牛第二个达到右边的端点，但实际上是最右边的奶牛此时到达右端点，依此类推。故我们在计算时间 $T$ 的时候，需按此统计停止移动的奶牛重量之和。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 5e4 + 10;
struct node {
    int w, x, d;
};
node A[N], B[N];
int n, l;

int main() {
    cin >> n >> l;
    int sum = 0;
    for (int i = 1; i <= n; i++) cin >> A[i].w >> A[i].x >> A[i].d, sum += A[i].w;
    sort(A + 1, A + n + 1, [](const node &a, const node &b) {
        if (a.d == -1 && b.d == -1) return a.x < b.x;
        if (a.d == -1 && b.d == 1) return a.x < l - b.x;
        if (a.d == 1 && b.d == -1) return l - a.x < b.x;
        return l - a.x < l - b.x;
    });

    for (int i = 1; i <= n; i++) B[i] = A[i];
    sort(B + 1, B + n + 1, [](const node &a, const node &b) {
        return a.x < b.x;
    });

    // 这里开始和子任务1 出现了区别，需要单独计算 T 的值
    int now = 0, t;
    int a = 1, b = n;
    for (int i = 1; i <= n; i++) {
        if (A[i].d == 1) now += B[b--].w;
        else now += B[a++].w;
        if (now * 2 >= sum) {
            t = A[i].d == 1 ? l - A[i].x : A[i].x;
            break;
        }
    }

    int cnt = 0;
    for (int i = 1; i <= n; i++) {
        for (int j = i + 1; j <= n; j++) {
            if (A[i].d == 1 && A[j].d == -1 && A[i].x <= A[j].x && A[i].x + 2 * t >= A[j].x) cnt++;
            if (A[i].d == -1 && A[j].d == 1 && A[j].x <= A[i].x && A[j].x + 2 * t >= A[i].x) cnt++;
        }
    }
    cout << cnt << endl;
}
```

## 一般情况

一般情况下，$n$ 到了 $5\times 10^4$ 级别，我们不能再枚举每对奶牛，一对奶牛 $(i, j)$ （ $i<j$）能够相遇，需要奶牛 $i$ 向右移动，奶牛 $j$ 向左移动，且两奶牛的距离不超过 $2T$，并且 $i, j$ 能够相遇，$i$，$j$ 之间方向向右的奶牛都能和 $j$ 相遇，如果 $i, j$ 不能相遇的话，那么 $i$ 与 $j$ 之后所有方向向左的奶牛都不能相遇，利用这一点，我们j将所有的奶牛按照位置从小到大排序，并维护一个队列，用来保存到目前位置所有方向向右的奶牛，如果当前位置奶牛的方向向右，我们将其放入队列，如果当前位置奶牛方向向左，我们检查队首元素与当前奶牛的距离，若超过 $2t$ 的话，则将队首元素弹出。不断进行该操作，直到满足条件。此时队列中的所有奶牛便是能够与奶牛 $j$ 相遇的奶牛。

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 5e4 + 10;
struct node {
  int w, x, d;
};
node A[N], B[N];
int n, l;

int main() {
  cin >> n >> l;
  int sum = 0;
  for (int i = 1; i <= n; i++) cin >> A[i].w >> A[i].x >> A[i].d, sum += A[i].w;
  sort(A + 1, A + n + 1, [](const node &a, const node &b) {
    if (a.d == -1 && b.d == -1) return a.x < b.x;
    if (a.d == -1 && b.d == 1) return a.x < l - b.x;
    if (a.d == 1 && b.d == -1) return l - a.x < b.x;
    return l - a.x < l - b.x;
  });

  for (int i = 1; i <= n; i++) B[i] = A[i];
  sort(B + 1, B + n + 1, [](const node &a, const node &b) {
    return a.x < b.x;
  });

  int now = 0, t;
  int a = 1, b = n;
  for (int i = 1; i <= n; i++) {
    if (A[i].d == 1) now += B[b--].w;
    else now += B[a++].w;
    if (now * 2 >= sum) {
      t = A[i].d == 1 ? l - A[i].x : A[i].x;
      break;
    }
  }

  // 此处开始和子任务2 的代码出现区别
  sort(A + 1, A + n + 1, [](const node &a, const node &b) {
    return a.x < b.x;
  });

  int cnt = 0;
  queue<int> q;
  for (int j = 1; j <= n; j++) {
    if (A[j].d == 1) {
    q.push(A[j].x);
    continue;
    }
    while (q.size() && q.front() + 2 * t < A[j].x) q.pop();
    cnt += q.size();
  }

  cout << cnt << endl;
}
```





