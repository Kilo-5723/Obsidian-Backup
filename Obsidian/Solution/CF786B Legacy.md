---
dg-publish: true
source: Codeforces
contest: Codeforces Round 406 (Div. 1)
cid: "786"
problem: B
name: Legacy
tags:
  - draft
  - segment-tree-graph
lead: 线段树建图模板
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

图中有 $n$ 个点，可以使用 $q$ 个操作实现图上的移动，操作可能为以下三种：
1. 从 $u$ 移动到 $v$，代价为 $w$
2. 从任意 $[l,r]$ 中的点到 $v$，代价为 $w$
3. 从 $u$ 到任意 $[l,r]$ 中的点，代价为 $w$

一开始位于 $s$ 点，求 $s$ 到各个点的最小代价，不可达则输出 $-1$。

数据范围 $n,q\le 10^5$。
### 题解

线段树建图模板

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
// segment tree graph that initial with node n
struct segraph {
  vector<int> a;
  int n, rt;  // in case of |a| = 1
  vector<int> ls, rs;
  segraph(vector<int> a, int n) : a(a), n(n) {
    ls.reserve(2 * a.size()), rs.reserve(2 * a.size());
    rt = build(0, a.size());
  }
  vector<pair<int, int>> e;
  int vsize() { return ls.size(); }
  int nnode() {
    ls.push_back(0), rs.push_back(0);
    return ls.size() - 1;
  }
  static int mid(int l, int r) { return (l + r) >> 1; }
  int build(int l, int r) {
    if (l + 1 == r) return a[l] - n;
    int u = nnode();
    ls[u] = build(l, mid(l, r)), rs[u] = build(mid(l, r), r);
    return u;
  }
  vector<pair<int, int>> edge() {
    vector<pair<int, int>> res;
    for (int u = 0; u < vsize(); u++)
      res.push_back({u + n, ls[u] + n}), res.push_back({u + n, rs[u] + n});
    return res;
  }
  vector<int> covers(int l, int r) {
    vector<int> res;
    auto dfs = [&](auto &&self, int u, int vl, int vr, int ql, int qr) -> void {
      if (qr <= vl || vr <= ql) return;
      if (ql <= vl && vr <= qr) {
        res.push_back(u + n);
        return;
      }
      self(self, ls[u], vl, mid(vl, vr), ql, qr);
      self(self, rs[u], mid(vl, vr), vr, ql, qr);
    };
    dfs(dfs, rt, 0, a.size(), l, r);
    return res;
  }
};
const ll inf = 1e18;
struct graph {
  vector<vector<pair<int, ll>>> e;
  graph(int n) : e(n + 1) {}
  void adde(int u, int v, ll w) { e[u].push_back({v, w}); }
  vector<ll> dijkstra(int s) {
    int n = e.size();
    vector<ll> dis(n, inf);
    priority_queue<pair<ll, int>, vector<pair<ll, int>>, greater<pair<ll, int>>>
        que;
    que.push({dis[s] = 0, s});
    while (que.size()) {
      auto [du, u] = que.top();
      que.pop();
      if (du > dis[u]) continue;
      for (auto [v, w] : e[u])
        if (dis[v] > du + w) que.push({dis[v] = du + w, v});
    }
    return dis;
  }
};
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, q, s;
  cin >> n >> q >> s;
  vector<int> a(n);
  iota(a.begin(), a.end(), 1);
  segraph g1(a, n + 1), g2(a, n + g1.vsize() + 1);
  graph g(n + g1.vsize() + g2.vsize());
  for (auto [u, v] : g1.edge()) g.adde(u, v, 0);
  for (auto [v, u] : g2.edge()) g.adde(u, v, 0);
  while (q--) {
    int op;
    cin >> op;
    if (op == 1) {
      int u, v, w;
      cin >> u >> v >> w;
      g.adde(u, v, w);
    } else if (op == 2) {
      int u, l, r, w;
      cin >> u >> l >> r >> w;
      for (auto v : g1.covers(l - 1, r)) g.adde(u, v, w);
    } else {
      int v, l, r, w;
      cin >> v >> l >> r >> w;
      for (auto u : g2.covers(l - 1, r)) g.adde(u, v, w);
    }
  }
  auto ans = g.dijkstra(s);
  for (int i = 1; i <= n; i++) cout << (ans[i] < inf / 2 ? ans[i] : -1) << ' ';
  cout << el;
  return 0;
}
```