## 线性同余方程组
方程组的形式为

$$
\left\{\begin{array}{c}
x \equiv a_{1}\left(\bmod m_{1}\right) \\
x \equiv a_{2}\left(\bmod m_{2}\right) \\
\vdots \\
x \equiv a_{n}\left(\bmod m_{n}\right)
\end{array}\right.
$$

假设已经求出了前 $k-1$ 个方程构成的方程组的一个解 $x$ 。记 $m=\operatorname{lcm}\left(m_{1}, m_{2}, \cdots\right.$, $m_{k-1}$ ), 则 $x+i * m(i \in \mathbb{Z})$ 是前 $k-1$ 个方程的通解。

考虑第 $k$ 个方程, 求出一个整数 $t$, 使得 $x+t \times m \equiv a_{k}\left(\bmod m_{k}\right)$ 。该方程等价 于 $m \times t \equiv a_{k}-x\left(\bmod m_{k}\right)$, 其中 $t$ 是末知量。这就是一个线性同余方程, 可以用扩展欧几里得算法判断是否有解, 并求出它的解。若有解, 则 $x^{\prime}=x+t \times m$ 就是前 $k$ 个方程构成的方程组的一个解。

综上所述, 我们使用 $n$ 次扩展欧几里得算法, 就求出了整个方程组的解。

```c++
x0 = 0, M = 1;
for (int i = 1; i <= n; i++) {
    int x, y, d;
    d = exgcd(M, m[i], x, y);

    if ((a[i] - x0) % d != 0) {
        cout << -1;
        return 0;
    }

    x0 = x * (a[i] - x0) / d % (m[i] / d) * M + x0;
    M = M / d * m[i];
    x0 %= M;
}
cout << (x0 % M + M) % M << endl;
```

## 中国剩余定理（Chinese Remainder Theorem）
>中国剩余定理通过构造得到的方程组的一个特解。
>设 $m_{1}, m_{2}, \cdots, m_{n}$ 是两两互质的整数。

对于每一个方程，我们找到一个特解，该特解是其余方程模数的倍数，这样，我们将每个方程的特解加在一起，便是方程组的解。

一般的，我们记 $m=\prod_{i=1}^{n} m_{i}$, $M_{i}=m / m_{i}$。

$t_{i}$ 是线性同余方程 $M_{i} x \equiv 1\left(\bmod m_{i}\right)$ 的一个解。

对于任意的 $n$ 个整数 $a_{1}, a_{2}, \cdots, a_{n}$, 方程组
$$
\left\{\begin{array}{c}
x \equiv a_{1}\left(\bmod m_{1}\right) \\
x \equiv a_{2}\left(\bmod m_{2}\right) \\
\vdots \\
x \equiv a_{n}\left(\bmod m_{n}\right)
\end{array}\right.
$$
有整数解, 解为 $x=\sum_{i=1}^{n} a_{i} M_{i} t_{i}$ 。
```c++
LL CRT() {
    LL T = 1, ans = 0;
    for (int i = 1; i <= n; i++) T = T * m[i];
    for (int i = 1; i <= n; i++) {
        LL M = T / m[i], t, y;
        exgcd(M, m[i], t, y); // b * m mod m[i] = 1
        ans = (ans + a[i] * M * t % mod) % mod;
    }
    return (ans % mod + mod) % mod;
}
```
<a href="/problem/14/testdata/download/%E7%BA%BF%E6%80%A7%E5%90%8C%E4%BD%99%E6%96%B9%E7%A8%8B%E7%BB%84%E5%92%8C%E4%B8%AD%E5%9B%BD%E5%89%A9%E4%BD%99%E5%AE%9A%E7%90%86%E7%BB%83%E4%B9%A0.docx">线性同余方程组和中国剩余定理练习题</a>