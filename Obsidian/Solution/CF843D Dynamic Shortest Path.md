---
dg-publish: true
source: Codeforces
contest: AIM Tech Round 4 (Div. 1)
cid: "843"
problem: D
name: Dynamic Shortest Path
tags:
  - graph
  - bucket-dijkstra
  - reweighting
lead: 每次修改做一次最短路图边权重构，即可限制最短路长度，用桶代替堆将复杂度降低 log n
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

给出一个正权有向图 $G=(V,E)$，需要维护两种操作：
1. 给出结点 $v$，查询 $1$ 到 $v$ 的距离
2. 给出 $k$ 条边，将 $k$ 条边的权值 $+1$

操作次数 $q\le 2000$，$|V|, |E|\le 10^5$，$\sum k\le 10^6$。

### 题解

如果暴力用堆优化 Dijkstra 计算最短路，复杂度为 $O(q|E|\log|V|)$，不能接受。

对最初的图进行边权重构，重构后的图中 $1$ 到所有点的距离都是 $0$。此时进行操作 2，可以发现所有点的最远距离不超过 $k$。因此，此时的 Dijkstra 可以用 $k$ 个桶代替堆，复杂度降低到 $O(\sum k+q|E|)$，整体复杂度为 $O(|E|\log|V|+\sum k+q|E|)$。

### 注意事项

这道题 **非常卡常**，如果想要通过这道题需要注意以下几点：
1. 仅在查询前有未处理过的边时才一起做桶优化 Dijkstra，这样可以把求最短路的次数减少到 $1000$ 次
2. 图的存储方式。在这道题里前向星比 `vector<vector<edge>>` 快非常多，这可能是因为输入中边是按顺序给出的。下面的实现中将边按起点排序存在数组里
3. 实践时发现，重构后根据高度更新边权比使用时根据高度动态计算边权速度更快

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const int lim = 1e6 + 5;
const ll inf = 1e18;
struct edge {
  int v;
  ll w;
};
struct graph {
  vector<int> head;
  vector<edge> e;
  vector<ll> h, d;  // h(u) and dis(s,u)
  graph(int n) : head(n + 2), h(n + 1), d(n + 1), que(lim) {}
  // return index of edges after sorting
  vector<int> init(vector<tuple<int, int, int>>& ed) {
    vector<int> res(ed.size());
    e.resize(ed.size());
    for (auto& [u, v, w] : ed) head[u]++;
    for (int i = 1; i < head.size(); i++) head[i] += head[i - 1];
    for (int i = 0; i < ed.size(); i++) {
      auto& [u, v, w] = ed[i];
      res[i] = --head[u];
      e[head[u]] = {v, w};
    }
    return res;
  }
  // initialize h(u)
  void dijkstra() {
    priority_queue<pair<ll, int>, vector<pair<ll, int>>, greater<pair<ll, int>>>
        que;
    fill(h.begin(), h.end(), inf);
    que.push({h[1] = 0, 1});
    while (que.size()) {
      auto [d, u] = que.top();
      que.pop();
      if (h[u] < d) continue;
      for (int i = head[u]; i < head[u + 1]; i++) {
        auto& [v, w] = e[i];
        auto dv = d + w;
        if (h[v] > dv) que.push({h[v] = dv, v});
      }
    }
    for (int u = 1; u < h.size(); u++)
      for (int i = head[u]; i < head[u + 1]; i++) e[i].w += h[u] - h[e[i].v];
  }
  vector<vector<int>> que;
  void update(int k) {
    fill(d.begin(), d.end(), k);
    que[d[1] = 0].push_back(1);
    for (int du = 0; du < k; du++) {
      while (que[du].size()) {
        auto u = que[du].back();
        que[du].pop_back();
        if (d[u] < du) continue;
        for (int i = head[u]; i < head[u + 1]; i++) {
          auto& [v, w] = e[i];
          auto dv = du + w;
          if (d[v] > dv) que[d[v] = dv].push_back(v);
        }
      }
    }
    for (int u = 1; u < d.size(); u++)
      for (int i = head[u]; i < head[u + 1]; i++) e[i].w += d[u] - d[e[i].v];
    for (int i = 1; i < d.size(); i++) h[i] += d[i];
  }
};
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, m, q;
  cin >> n >> m >> q;
  graph g(n);
  vector<tuple<int, int, int>> ed(m);
  for (auto& [u, v, w] : ed) cin >> u >> v >> w;
  auto pos = g.init(ed);
  g.dijkstra();  // initialize h(u) with dijkstra
  int k = 0;
  while (q--) {
    int op;
    cin >> op;
    if (op == 1) {
      int v;
      cin >> v;
      if (k) g.update(k), k = 0;
      cout << (g.h[v] < inf / 2 ? g.h[v] : -1) << el;  // h(u) is distance
    } else {
      int t;
      cin >> t;
      k += t;
      for (int i = 0; i < t; i++) {
        int v;
        cin >> v;
        g.e[pos[v - 1]].w++;
      }
    }
  }
  return 0;
}
```