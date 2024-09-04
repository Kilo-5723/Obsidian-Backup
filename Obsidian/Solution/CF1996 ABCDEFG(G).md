---
dg-publish: true
source: Codeforces
contest: Codeforces Round 962 (Div. 3)
cid: "1996"
problem: G
name: Problem A ~ G (G)
tags:
  - multiple
lead: (A) 输出 (B) 输出 (C) 前缀和 (D) 根号枚举 (E) 前缀和+计数 (F) wqs 二分青春版 (G) 线段树/哈希
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.collect_header(dv);
```

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "A", "Legs");
```
#naive

答案是 $\lfloor (n+2)/4 \rfloor$。 [[#^a2|Code]] ^a1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "B", "Scale");
```
#naive 

输出所有 $i\bmod k=j\bmod k=0$ 的 $a_{i,j}$。 [[#^b2|Code]] ^b1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "C", "Sort");
```
#prefix-sum 

对 $26$ 个字母分别计算前缀和，一个区间的修改代价就是所有字母出现次数之差的绝对值之和除以 $2$。 [[#^c2|Code]] ^c1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "D", "Fun");
```
#enumeration  #harmonic-series

注意到次大数不会超过 $\sqrt{n}$。三个数都相同的情况有 $\lfloor\min(\sqrt{n/3},x/3)\rfloor$ 种，两个数相同的情况可以枚举相同的数，三个数不同的情况可以枚举最小数和次大数，复杂度为 $O(n)$。 [[#^d12|Code]] ^d11

也可以直接枚举 $a$ 和 $b$。注意到 $b\le n/a$，所以 $(a,b)$ 总共有 $O(n\log n)$ 对，$c$ 可以直接计算，复杂度为 $O(n\log n)$。 [[#^d22|Code]] ^d21

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "E", "Decode");
```
#prefix-sum #counting 

将原题的计数改为对每个 `01` 数量相同的子串求有多少串包含它。考虑如何快速找出 `01` 数量相同的子串。令 $a_i$ 为第 $i$ 位的权重，`0` 和 `1` 的权重分别为 $-1$ 和 $1$，则对任意满足条件的子串 $s_l\dots s_r$ 都有 $\sum_{i=1}^{l-1} a_i=\sum_{i=1}^r a_i$。将 $0\le p\le n$ 按 $\sum_{i=1}^p a_i$ 分类，不妨假设某一类是 $[p_1,p_2,\dots,p_k]$，则对任意 $i<j$，$s_{p_{i+1}}\dots s_{p_j}$ 都满足要求。

接下来考虑如何快速统计每一类 $[p_1,\dots,p_k]$ 对答案的贡献。子串 $s_l,\dots,s_r$ 对答案的贡献是 $l\cdot (n-r+1)$，要快速求 $\sum_{i=1}^k \left(p_i\cdot \sum_{j=i+1}^k (n-p_j+1)\right)$ ，只需要先对 $n-p_j+1$ 求和，顺序枚举 $i$ 并依次将 $n-p_i+1$ 减去。 [[#^e2|Code]] ^e1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "F", "Bomb");
```
#laglange-multiplyer 

在执行 $k$ 次操作时，每次都一定会选择当前收益最大的操作，且每次操作的收益递减。因此，可以二分最后一次操作的收益 $v$，得到操作收益不低于 $v$ 时操作的次数 ${\rm cnt}(v)$ 和所有操作的总收益 ${\rm sum}(v)$。二分找到最小的 $v\ge 0$ 使得 ${\rm cnt}(v+1)<k$，答案就是 ${\rm sum}(v)-({\rm cnt}(v)-k)\cdot v$。 [[#^f2|Code]] ^f1

```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_single(dv, "G", "Penacony");
```
#data-structure #hashing #differential #prefix-sum

对于每对 $(a,b)$，其在环上可能沿两条路径连通，但如果能确定一条边不存在，就可以唯一确定一条路径。把环上的边视为一个序列，则删去一条边后 $(a,b)$ 的限制相当于序列上的一个区间 $[a',b')$ 必须保留。考虑每条边被形如 $[a',b')$ 的区间覆盖的次数，可以删除的边就是被覆盖次数为 $0$ 的边。

接下来，考虑当确定删除的边从 $(i-1,i)$ 变成 $(i,i+1)$ 时，$(a,b)$ 对应的区间 $[a',b)$ 会如何变化。先将 $(i-1,i)$ 移到序列右端，可以发现如果 $(a,b)$ 的端点不为 $i$，则其对应的区间不会发生任何变化，否则其对应的区间会从 $[a',b')$ 变为 $[b',a'+n)$，相当于对 $[a',b')$ 内的边的覆盖次数减 $1$，对 $[b',a'+n)$ 加 $1$。

考虑数据结构维护每条边被区间覆盖的次数，则需要支持的操作有：
- 区间加减 $1$
- 查询区间中 $0$ 出现的次数，保证区间所有数 $\ge 0$。

这可以用一个维护区间最小值及其出现次数的线段树实现。 [[#^g12|Code]] ^g11

另外一种做法是观察上面操作的本质，发现对于两条边 $(i,i+1), (j,j+1)$，当且仅当这两条边对所有 $(a,b)$ 都在环上的同一侧时才能同时删去。

用哈希求可以同时删去的边集。对每对 $(a,b)$ 随机生成一个哈希值 $h(a,b)$ 并异或到 $(a,b)$ 一条路径上的每条边 $h(i)$。这样，如果两条边 $i,j$ 在所有 $(a,b)$ 的同侧就一定有 $h(i)=h(j)$，哈希值相同的集合就是可以同时删去的边集。可以用差分与前缀和在 $O(n)$ 的时间复杂度算出 $h(i)$，总复杂度为 $O(n\log n)$。 [[#^g22|Code]] ^g21

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
    cout << (n + 2) / 4 << el;
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
    int n, k;
    cin >> n >> k;
    vector<string> a(n);
    for (auto &s : a) cin >> s;
    for (int i = 0; i < n; i += k, cout << el)
      for (int j = 0; j < n; j += k) cout << a[i][j];
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
    int n, q;
    cin >> n >> q;
    vector<array<int, 26>> a(n + 1);
    string s;
    cin >> s;
    for (int i = 0; i < n; i++) a[i + 1][s[i] - 'a']++;
    cin >> s;
    for (int i = 0; i < n; i++) a[i + 1][s[i] - 'a']--;
    for (int i = 1; i <= n; i++)
      for (int j = 0; j < 26; j++) a[i][j] += a[i - 1][j];
    while (q--) {
      int l, r;
      cin >> l >> r;
      int ans = 0;
      for (int i = 0; i < 26; i++) ans += abs(a[r][i] - a[l - 1][i]);
      cout << ans / 2 << el;
    }
  }
  return 0;
}
```

[[#^d11|Problem D]] ^d12

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
ll _sqrt(ll n) {
  ll m = sqrt(n);
  while (m * m <= n) m++;
  while (m * m > n) m--;
  return m;
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    ll n, x;
    cin >> n >> x;
    ll k = _sqrt(n);
    ll ans = -2 * min(x / 3, _sqrt(n / 3));
    for (ll a = 1; a * a <= n; a++)
      ans += 3 * max(min((n - a * a) / 2 / a, x - 2 * a), 0ll);
    for (ll m = 1; m <= k; m++)
      for (ll l = 1; l < m; l++) {
        ll up = min((n - l * m) / (l + m), x - l - m);
        ans += 6 * max(0ll, up - m);
      }
    cout << ans << el;
  }
  return 0;
}
```

[[#^d21|Problem D]] ^d22

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
    ll n, x;
    cin >> n >> x;
    ll ans = 0;
    for (int i = 1; i <= n; i++)
      for (int j = 1; j * i <= n; j++)
        ans += max(min((n - i * j) / (i + j), x - i - j), 0ll);
    cout << ans << el;
  }
  return 0;
}
```

[[#^e1|Problem E]] ^e2

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
    string s;
    cin >> s;
    int n = s.size();
    map<int, vector<int>> pos;
    pos[0].push_back(0);
    for (int i = 0, cur = 0; i < n; i++) {
      cur += (s[i] - '0') * 2 - 1;
      pos[cur].push_back(i + 1);
    }
    ll ans = 0;
    for (auto [k, arr] : pos) {
      ll sum = 0;
      for (auto v : arr) sum += n - v + 1;
      for (auto v : arr) sum -= n - v + 1, ans += sum % mod * (v + 1) % mod;
    }
    cout << (ans % mod + mod) % mod << el;
  }
  return 0;
}
```

[[#^f1|Problem F]] ^f2

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const ll inf = 1e18;
pair<ll, ll> solve(vector<ll> a, vector<ll> b, ll low) {
  int n = a.size();
  ll cnt = 0, sum = 0;
  for (int i = 0; i < n; i++)
    if (low <= a[i]) {
      ll k = (a[i] - low) / b[i] + 1;
      cnt += k;
      sum += k * a[i] - k * (k - 1) / 2 * b[i];
    }
  return {cnt, sum};
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n, k;
    cin >> n >> k;
    vector<ll> a(n), b(n);
    for (auto &v : a) cin >> v;
    for (auto &v : b) cin >> v;
    ll l = 0, r = inf;
    while (r - l > 1) {
      ll m = l + (r - l) / 2;
      if (solve(a, b, m).first >= k)
        l = m;
      else
        r = m;
    }
    auto [cnt, sum] = solve(a, b, l);
    cout << sum - l * (cnt - k) << el;
  }
  return 0;
}
```

[[#^g11|Problem G Sol. 1]] ^g12

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
struct node {
  int l, r;
  node *ls, *rs;
  int min, cnt;
  int tag;
  node() : l(0), r(0), ls(nullptr), rs(nullptr), min(0), cnt(0), tag(0) {}
  static void *operator new(size_t count) {
    static node *begin = nullptr, *end = nullptr;
    if (begin == end) begin = (node *)malloc(count * 1000), end = begin + 1000;
    return begin++;
  }
};
void update(node *u) {
  u->min = min(u->ls->min, u->rs->min);
  u->cnt = 0;
  for (auto ch : {u->ls, u->rs})
    if (u->min == ch->min) u->cnt += ch->cnt;
}
void addtag(node *u, int t) {
  u->tag += t;
  u->min += t;
}
void pushdown(node *u) {
  addtag(u->ls, u->tag), addtag(u->rs, u->tag);
  u->tag = 0;
}
node *build(int l, int r) {
  node *u = new node();
  u->l = l, u->r = r;
  if (u->l == u->r) {
    u->min = 0, u->cnt = 1;
    return u;
  }
  int m = l + (r - l >> 1);
  u->ls = build(l, m), u->rs = build(m + 1, r);
  update(u);
  return u;
}
void add(node *u, int l, int r, int v) {
  if (r < u->l || u->r < l) return;
  if (l <= u->l && u->r <= r) {
    addtag(u, v);
    return;
  }
  pushdown(u);
  add(u->ls, l, r, v), add(u->rs, l, r, v);
  update(u);
  return;
}
int query(node *u, int l, int r) {
  if (r < u->l || u->r < l) return 0;
  if (l <= u->l && u->r <= r) return (!u->min) * u->cnt;
  pushdown(u);
  return query(u->ls, l, r) + query(u->rs, l, r);
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n, m;
    cin >> n >> m;
    node *rt = build(1, 2 * n);
    vector<vector<int>> e(n + 1);
    while (m--) {
      int u, v;
      cin >> u >> v;
      add(rt, u, v - 1, 1);
      e[u].push_back(v);
      e[v].push_back(u + n);
    }
    int ans = query(rt, 1, n);
    for (int i = 1; i < n; i++) {
      for (auto j : e[i]) add(rt, i, j - 1, -1), add(rt, j, i + n - 1, 1);
      ans = max(ans, query(rt, 1 + i, n + i));
    }
    cout << n - ans << el;
  }
  return 0;
}
```

[[#^g21|Problem G Sol. 2]] ^g22

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
typedef unsigned long long ull;
random_device rnd;
default_random_engine eng(rnd());
uniform_int_distribution<ull> gen;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n, m;
    cin >> n >> m;
    vector<ull> a(n);
    while (m--) {
      int u, v;
      cin >> u >> v;
      ull h = gen(eng);
      a[u - 1] ^= h, a[v - 1] ^= h;
    }
    for (int i = 1; i < n; i++) a[i] ^= a[i - 1];
    map<ull, int> cnt;
    int ans = 0;
    for (auto v : a) cnt[v]++;
    for (auto [k, v] : cnt) ans = max(ans, v);
    cout << n - ans << el;
  }
  return 0;
}
```

