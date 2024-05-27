![image-20231109191037708](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20231109191037708.png)

首先我们查找从字符串第一个字符串开始的单词，假设有两个单词，结束位置分别位于 $3$ 和 $6$ 。

![image-20231109192759861](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20231109192759861.png)

接下来我们查找从 $4$ 号位置开始的单词，分别给结束位置打上标记。

![image-20231109193121693](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20231109193121693.png)

接下来，我们找到第二个标记的位置 $5$，查找从 $5$ 号位置开始的单词，给结束位置 $12$ 打上标记。

依此类推，直到遍历完所有标记过的位置。

![image-20231109192936997](C:\Users\Lee\OneDrive\笔记\算法学习\题解\assets\image-20231109192936997.png)

问题的答案便是标记位置的最大值。

```c++
#include <bits/stdc++.h>
using namespace std;

int trie[205][26], tot = 1;
bool End[205];
int n, m;

void insert(string str) {
    int p = 1;
    for (const auto &ch : str) {
        int t = ch - 'a';
        if (trie[p][t] == 0) trie[p][t] = ++tot;
        p = trie[p][t];
    }
    End[p] = true;
}

bool marked[2000000];
int search(string str) {
    memset(marked, 0, sizeof marked);
    marked[0] = true;
    int len = str.length();
    str = '#' + str;
    int ans = 0;
   
    for (int i = 0; i <= len; i++) {
        if (!marked[i]) continue;
        int p = 1;
        for (int j = i + 1; j <= len; j++) {
            int t = str[j] - 'a';
            if (trie[p][t] == 0) break;
            p = trie[p][t];
            if (End[p]) marked[j] = true, ans = max(ans, j);
        }
    }
    return ans;
}

int main() {
    cin >> n >> m;
    for (int i = 1; i <= n; i++) {
        string str;
        cin >> str;
        insert(str);
    }
    for (int i = 1; i <= m; i++) {
        string str;
        cin >> str;
        cout << search(str) << endl;
    }
}
```

