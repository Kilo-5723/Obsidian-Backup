---
dg-publish: true
source: Codeforces
contest: VK Cup 2015 - Round 3 (Div. 1)
cid: "542"
problem: A
name: Place Your Ad Here
tags:
  - monotone-structure
  - draft
lead: 用 map 维护两个从中间插入的单调队列
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

对着带权区间 $[a,b], c$ 找不带权区间 $[l,r]$，发现一共有三种情况：
1. $l<a$，此时相交区间为 $[a,\min(r,b)]$
2. $a\le l$ 且 $r<b$，此时相交区间为 $[l,r]$
3. $a\le l$ 且 $b\le r$，此时相交区间为 $[l,b]$

注意到 $l=a$ 满足 $l<a$ 的讨论，$r=b$ 也满足 $r<b$ 的讨论。因此，下面的讨论不需要排除相等的情况。

将全部区间按左端点降序排序并分别处理三种情况。令当前左端点为 $l'$，则：
1. 对于 $l<a$，用 `set` 维护所有 $l<l'$ 的区间。处理 $[a,b],c$ 时查询 `set` 中最大的右端点，处理 $[l,r]$ 时将区间从 `set` 中删去
2. 对于 $a\le l$ 且 $r<b$，用 `map` 维护一个单调队列，其等价于一个递增函数 $f(r')$，其值为所有 $[l,r]\subseteq[l',r']$ 的区间中最大的 $r-l$。处理 $[a,b],c$ 时查询 $f(b)$，处理 $[l,r]$ 时将 $f(r)=r-l$ 插入单调队列
3. 对于 $a\le l$ 且 $b\le r$，用 `map` 维护一个单调队列，其等价于一个递增函数 $f(r')$，其值为所有 $l'\le l,r'\le r$ 的区间中最小的 $l$。处理 $[a,b],c$ 时查询 $f(b)$，处理 $[l,r]$ 时将 $f(r)=l$ 插入单调队列

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const int inf = 1.01e9;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, m;
  cin >> n >> m;
  vector<tuple<int, int, ll, int>> a(n + m);
  for (int i = 0; i < n; i++) {
    auto &[l, r, w, id] = a[i];
    cin >> l >> r, w = 0, id = i + 1;
  }
  for (int i = n; i < n + m; i++) {
    auto &[l, r, w, id] = a[i];
    cin >> l >> r >> w, id = i - n + 1;
  }
  sort(a.begin(), a.end()), reverse(a.begin(), a.end());
  set<pair<int, int>> rhs = {{-inf, -1}};
  for (auto [l, r, w, id] : a)
    if (!w) rhs.insert({r, id});
  ll ans = 0, i, j;
  map<int, pair<int, int>> lhs = {{-inf, {-inf, -1}}, {inf, {inf, -1}}};
  map<int, pair<int, int>> eq = {{-inf, {-inf, -1}}, {inf, {inf, -1}}};
  auto insert = [&](int l, int r, int id) {
    auto it1 = lhs.lower_bound(r);
    if (it1->second > make_pair(l, id)) {
      lhs[r] = {l, id}, it1 = lhs.find(r);
      while (prev(it1)->second > make_pair(l, id)) lhs.erase(prev(it1));
    }
    auto it2 = prev(eq.upper_bound(r));
    if (it2->second < make_pair(r - l, id)) {
      eq[r] = {r - l, id}, it2 = eq.find(r);
      while (next(it2)->second < make_pair(r - l, id)) eq.erase(next(it2));
    }
    rhs.erase({r, id});
  };
  auto query =
      [&](int r) -> tuple<pair<int, int>, pair<int, int>, pair<int, int>> {
    auto it1 = lhs.lower_bound(r), it2 = prev(eq.upper_bound(r));
    return {it1->second, it2->second, *rhs.rbegin()};
  };
  for (auto [l, r, w, id] : a)
    if (w) {
      auto [lhs, eq, rhs] = query(r);
      if (ans < (r - lhs.first) * w)
        ans = (r - lhs.first) * w, i = lhs.second, j = id;
      if (ans < eq.first * w) ans = eq.first * w, i = eq.second, j = id;
      if (ans < (min(r, rhs.first) - l) * w)
        ans = (min(r, rhs.first) - l) * w, i = rhs.second, j = id;
    } else {
      insert(l, r, id);
    }
  cout << ans << el;
  if (ans) cout << i << ' ' << j << el;
  return 0;
}
```