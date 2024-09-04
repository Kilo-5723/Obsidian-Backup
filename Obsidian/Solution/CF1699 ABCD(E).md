---
dg-publish: true
source: Codeforces
contest: Codeforces Round 804 (Div. 2)
cid: "1699"
problem: D
name: Problem A ~ D (E)
tags:
  - multiple
lead: (A) 构造 (B) 构造 (C) mex 计数 (D) 动态规划
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.collect_header(dv);
```

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "A", "The Third Three Number Problem");
```
#construction 

- $n=2k+1$：$a\oplus b + b\oplus c+a\oplus c$ 的末位不可能是 $1$，故此时无解。
- $n=2k$：一组满足要求的 $a,b,c$ 为 $k,0,0$。 [[#^a2|Code]] ^a1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "B", "Almost Ternary Matrix");
```
#construction 

注意到 $n,m$ 为偶数。如果保证对任意 $k$ 都有 $2k+1$ 与 $2k+2$ 行/列相等，则可以将 $n\times m$ 的方格图分割成数个 $2\times 2$ 的小格，每个格子可能颜色不同的邻居都在小格内部。可以将问题规约为 $2\times 2$ 的情况，构造如下： [[#^b2|Code]] ^b1
```
x  .  .  x  x  .  .  x
.  x  x  .  .  x  x  .
.  x  x  .  .  x  x  .
x  .  .  x  x  .  .  x
```


```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "C", "The Third Problem");
```
#counting 

对于任意一个数 $k$，因为 $k$ 只会对已经包含 $0$ 到 $k-1$ 所有数字的区间 $\rm mex$ 产生影响，因此如果 $a$ 中 $k$ 的位置在覆盖 $0$ 到 $k-1$ 的最小区间之内，则 $b$ 中可以将 $k$ 放在该区间内任意位置，否则必须摆放在 $a$ 中同一位置。从小到大枚举 $k$ 并维护当前区间即可。 [[#^c2|Code]] ^c1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "D", "Almost Triple Deletions");
```
#dynamic-programming 

考虑最后答案中的数字在原来的序列中的位置，则两个相邻数字之间的原序列必须全部被消除。

可以被消除的区间满足两个性质，区间长度为偶数，以及出现次数最多的数字不超过区间长度的一半。可以枚举区间起点，终点右移时维护当前各个数字出现次数，在 $O(n^2)$ 的时间里找出所有可以被消除的区间。

求出可以被消除的区间后，就只需要找到从左端到右端，只经过同一个数字的最长跳跃序列。这也可以在 $O(n^2)$ 的时间内 DP 求出。 [[#^d2|Code]] ^d1

[[#^a1|Problem A]] ^a2

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n;
    cin >> n;
    if (n & 1) {
      cout << -1 << el;
      continue;
    }
    cout << n / 2 << ' ' << 0 << ' ' << 0 << el;
  }
  return 0;
}
```

[[#^b1|Problem B]] ^b2

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++, cout << el)
      for (int j = 0; j < m; j++)
        cout << ((i & 1) ^ (j & 1) ^ (i >> 1 & 1) ^ (j >> 1 & 1)) << ' ';
  }
  return 0;
}
```

[[#^c1|Problem C]] ^c2

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const ll mod = 1e9 + 7;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n;
    cin >> n;
    vector<int> a(n);
    for (auto &v : a) cin >> v;
    vector<int> b(n);
    for (int i = 0; i < n; i++) b[a[i]] = i;
    int l = b[0], r = b[0];
    ll ans = 1;
    for (int i = 1; i < n; i++) {
      if (b[i] < l)
        l = b[i];
      else if (b[i] > r)
        r = b[i];
      else
        ans = ans * (r - l - i + 1) % mod;
    }
    cout << ans << el;
  }
  return 0;
}
```

[[#^d1|Problem D]] ^d2

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const int inf = 1e9;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n;
    cin >> n;
    vector<int> a(n);
    for (auto &v : a) cin >> v;
    vector<vector<int>> e(n + 1);
    for (int i = 0; i <= n; i++) {
      vector<int> cnt(n + 1);
      int mxm = 0;
      e[i].push_back(i - 1);
      for (int j = 0; i + j < n; j++) {
        mxm = max(mxm, ++cnt[a[i + j]]);
        if ((j & 1) && 2 * mxm <= j + 1) e[i].push_back(i + j);
      }
    }
    vector<int> dp(n + 2, -inf);
    dp[n + 1] = -1;
    for (int i = n; i >= 0; i--)
      for (auto j : e[i])
        if (i == 0 || j == n - 1 || a[i - 1] == a[j + 1])
          dp[i] = max(dp[i], dp[j + 2] + 1);
    cout << dp[0] << el;
  }
  return 0;
}
```
