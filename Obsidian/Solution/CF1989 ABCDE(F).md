---
dg-publish: true
source: Codeforces
contest: Educational Codeforces Round 167
cid: "1989"
problem: E
name: Problem A ~ E (F)
tags:
  - multiple
lead: (A) 结论 (B) 枚举+贪心 (C) 贪心 (D) DP+贪心 (E) 计数+前缀和优化 DP
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.collect_header(dv);
```

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "A", "Catch the Coin");
```
#conclusion

显然，当且仅当 $y\ge-1$ 时玩家能追上硬币。 [[#^a2|Code]] ^a1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "B", "Substring and Subsequence");
```
#enumeration #greedy

显然我们希望让 $a$ 和 $b$ 重复的字符尽可能多。因为 $a$ 必须连续出现，枚举 $b$ 的哪些部分在 $a$ 之前出现，后面就可以贪心匹配。 [[#^b2|Code]] ^b1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "C", "Two Movies");
```
#greedy 

如果一个人对两个电影的评价不一致，因为评分只有 $\{-1,0,1\}$ 三种情况，一定会贪心地让他对评价较高的电影投票。否则，根据当前两个电影的评分，一定会把好评给当前评价更差的电影，差评给当前评价最好的电影。 [[#^c2|Code]] ^c1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "D", "Smithing Skill");
```
#dynamic-programming #greedy 

显然每个 $c_i$ 是独立的，考虑 $f(x)$ 表示当前某种材料剩 $x$ 个时可获得的最大经验，则有：
$$f(x)=\max\{f(x-a_i+b_i)+2\mid x\ge a_i\}$$
对 $a_i$ 排序预处理 $x\ge a_i$ 时最小的 $a_i-b_i$，就可以在 $O(n)$ 的时间里求出 $f(x)$ 的前 $n$ 项。注意到 $x>2\cdot 10^6$ 时 $f(x)=f(x-\max\{a_i-b_i\})$，因此可以预处理 $f(x)$ 的前 $2\cdot 10^6$ 项再另外处理 $f(x)>2\cdot 10^6$ 的情况。 [[#^d2|Code]] ^d1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "E", "Distance to Different");
```
#dynamic-programming #counting #prefix-sum

我们考虑将 $a$ 分解为段，例如 $[1,1,2,3,3,3,3,1]\to [(1,2),(2,1),(3,4),(1,1)]$。可以发现，不管每段对应的数字是什么，只要和前后不一样就不会影响其对应的 $b$ 序列，而题目中 $k$ 的限制相当于要求 $a$ 至少有 $k$ 段。因此，对所有不同 $b$ 的计数可以转化为将长度为 $n$ 的序列分成至少 $k$ 段的方案数。

注意到当段长为 $2$ 且该段不在首尾出现时，其对应的 $b$ 无法和两段长为 $1$ 的段区分。因此，我们在计算时不允许段长为 $2$，将 $2$ 在首尾出现的情况另外讨论。令 $f(n,k)$ 为将长度为 $n$ 的序列分成至少 $k$ 段且每段段长不能为 $2$ 的方案数，则所求不同的 $b$ 序列的数量就是 $f(n,k)+2\cdot f(n-2,k-[k>0]) + f(n-4,k-2)$，其后两项对应 $2$ 在首尾出现的情况。同时，我们有：
$$\begin{align*}
f(n,k)&=\sum_{i=1}^{n}[i\neq 2] f(n-i,\max(k-[k>0])\\
&=\sum_{i=1}^n f(n-i,k-[k>0]) - f(n-2,k-[k>0])
\end{align*}$$
其可以通过维护前缀和在 $O(nk)$ 的时间复杂度内求出。 [[#^e2|Code]] ^e1

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
    int x, y;
    cin >> x >> y;
    cout << (y < -1 ? "NO" : "YES") << el;
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
int common(string a, string b) {
  int p = 0;
  for (auto v : a)
    if (p < b.size() && v == b[p]) p++;
  return p;
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    string a, b;
    cin >> a >> b;
    int ans = 0;
    for (int i = 0; i < b.size(); i++)
      ans = max(ans, common(a, {b.begin() + i, b.end()}));
    cout << a.size() + b.size() - ans << el;
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
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n, pos = 0, neg = 0, a = 0, b = 0;
    cin >> n;
    vector<pair<int, int>> arr(n);
    for (auto &[x, y] : arr) cin >> x;
    for (auto &[x, y] : arr) cin >> y;
    for (auto [x, y] : arr)
      if (x != y)
        x > y ? a += x : b += y;
      else
        x > 0 ? pos++ : x < 0 ? neg++ : 0;
    if (a > b) swap(a, b);
    if (a + pos < b - neg)
      cout << a + pos << el;
    else
      cout << (a + b + pos - neg >> 1) << el;
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
const int lim = 2.1e6;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, m;
  cin >> n >> m;
  vector<pair<int, int>> a(n);
  for (auto &[x, y] : a) cin >> y;
  for (auto &[x, y] : a) cin >> x, x = y - x;
  vector<int> b(m);
  for (auto &v : b) cin >> v;
  sort(a.begin(), a.end());
  a.resize(unique(a.begin(), a.end(),
                  [&](auto a, auto b) { return a.first == b.first; }) -
           a.begin());
  vector<pair<int, int>> tmp;
  for (auto v : a)
    if (!tmp.size() || tmp.back().second > v.second) tmp.push_back(v);
  swap(a, tmp);
  sort(b.begin(), b.end());
  vector<int> dp(lim);
  int p = a.size();
  for (int i = 0, j = 0; i < lim; i++) {
    while (p && a[p - 1].second <= i) p--;
    if (p < a.size()) dp[i] = dp[i - a[p].first] + 1;
  }
  auto get = [&](int n) {
    if (n < lim) return dp[n];
    auto step = (n - lim) / a[0].first + 1;
    return dp[n - step * a[0].first] + step;
  };
  ll ans = 0;
  for (auto v : b) ans += get(v);
  cout << ans * 2 << el;
  return 0;
}
```

[[#^e1|Problem E]] ^e2

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const ll mod = 998244353;
const int lowbit(int n) { return n & -n; }
struct bit {
  vector<ll> a, b;
  bit(int n) : a(n + 2), b(n + 2) {}
  void set(int v, ll k) {
    k -= b[++v];
    for (int i = v; i < a.size(); i += lowbit(i)) a[i] = (a[i] + k) % mod;
    b[v] = (b[v] + k) % mod;
  }
  ll get(int v) { return ++v > 0 ? b[v] : 0; }
  ll sum(int n) {
    if (++n <= 0) return 0;
    ll res = 0;
    for (int i = n; i; i -= lowbit(i)) res += a[i];
    return res % mod;
  }
};
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, k;
  cin >> n >> k;
  vector b(k + 1, bit(n));
  b[0].set(0, 1);
  for (int i = 1; i <= n; i++) {
    b[0].set(i, b[0].get(i - 1) + b[0].sum(i - 3));
    for (int j = 1; j <= k; j++)
      b[j].set(i, b[j - 1].get(i - 1) + b[j - 1].sum(i - 3));
  }
  ll ans = b[k - 2].get(n - 4) + 2 * b[k - 1].get(n - 2) + b[k].get(n);
  cout << (ans % mod + mod) % mod << el;
  return 0;
}
```