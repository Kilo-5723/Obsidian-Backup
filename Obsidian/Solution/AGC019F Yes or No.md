---
dg-publish: true
source: AtCoder
contest: AtCoder Grand Contest 019
cid: agc019
problem: F
name: Yes or No
tags:
  - probability
  - construction
lead: 对概率赋予组合意义，改变期望的求和形式
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

一个游戏，已知有 $n$ 题答案为 Yes，$m$ 题答案为 No，但顺序随机。

参赛者按照顺序回答每一个问题，每次回答完立刻知道回答正确与否，求回答正确次数的最大期望。

数据范围 $1\le n,m\le 10^5$。
### 题解

我们将正确答案视作一个 0-1 序列。参赛者每一步都知道剩下的序列有几个 0 和 1，而只有猜中时才会得分，则显然参赛者的策略是每次都回答剩下的数字里较多的一个。令当前剩下 $i$ 个 1 和 $j$ 个 0，则不妨假设参赛者的策略是 $i\ge j$ 时猜 1，$i<j$ 时猜 0。

我们把这个 0-1 序列看作在网格图上的折线，发现其贡献就是在 $i\ge j$ 的部分向下，即 $(-1,0)$ 方向走的次数与 $i<j$ 的部分向左，即 $(0,-1)$ 方向走的次数。可以发现 $i>j$ 向下走的次数和 $i<j$ 向左走的次数加起来一定等于 $\max(n,m)$，因此我们只需要求 $i=j$ 时向下走的次数期望。不妨令 $n<m$，则答案为：$$m+\sum_{i=1}^n\frac{C(2i,i)\cdot C(n+m-2i,n-i)}{2\cdot C(n+m,n)}$$
另外提供一个绕个弯子的思路。参赛者在剩下 $i$ 个 1 和 $j$ 个 0 时的期望得分是 $\frac{\max(i,j)}{i+j}$。考虑到达这种情况的概率 $\frac{C(n,i)\cdot C(m,j)}{C(n+m,i+j)}$，则这种情况对答案的贡献是：
$$ f(i,j)=\frac{\max(i,j)\cdot C(n,i)\cdot C(m,j)}{(i+j)\cdot C(n+m,i+j)} $$
当 $\max(i,j)=i$ 时，我们有：
$$ \begin{align*}
f(i,j)&= \frac{C(i,1)\cdot C(n,i)\cdot C(m,j)}{C(i+j,1)\cdot C(n+m,i+j)} \\
&= \frac{C(n,1)\cdot C(n-1,i-1)\cdot C(m,j)}{C(n+m,1)\cdot C(n+m-1,i+j-1)} \\
\end{align*}
$$
这可以理解为特定的 $i+j$ 位中有 $i$ 位是 1，且其中特定的一位是 1 的概率，我们令其为第 $i$ 个 1 恰好在第 $i+j$ 位的概率。考虑 $\max(i,j)=i$ 的限制，就变成了第 $i$ 个 1 在第 $i+j$ 位且 $j\le i$ 的概率。

考虑某个 0-1 序列对答案的总贡献，我们对每个 $k=i+j$ 判断上面情况成立的概率，就变成了第 $k$ 个数恰好是前 $k$ 个数中出现次数较多的数的总概率。为了防止对 $i=j$ 的情况重复计数，如果 0 和 1 出现次数相同，则认为 1 出现次数更多。

我们在网格图上考虑，发现其贡献就是在 $i\ge j$ 的部分向下走的次数与 $i<j$ 的部分向左走的次数，可以得出和上面相同的答案。


```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const ll mod = 998244353;
ll qpow(ll a, ll b) {
  ll res = 1;
  while (b) res = res * (b & 1 ? a : 1) % mod, a = a * a % mod, b >>= 1;
  return res;
}
ll inv(ll n) { return qpow(n, mod - 2); }
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, m;
  cin >> n >> m;
  vector<ll> fact(n + m + 1), ifac(n + m + 1);
  fact[0] = 1;
  for (int i = 1; i <= n + m; i++) fact[i] = fact[i - 1] * i % mod;
  ifac.back() = inv(fact.back());
  for (int i = n + m; i; i--) ifac[i - 1] = ifac[i] * i % mod;
  auto comb = [&](int n, int m) {
    return fact[n] * ifac[m] % mod * ifac[n - m] % mod;
  };
  if (n > m) swap(n, m);
  ll prob = 0;
  for (int i = 1; i <= n; i++)
    prob += comb(2 * i, i) * comb(n + m - 2 * i, n - i) % mod;
  prob = prob % mod * inv(comb(n + m, n)) % mod;
  cout << (m + prob * inv(2) % mod) << el;
  return 0;
}
```