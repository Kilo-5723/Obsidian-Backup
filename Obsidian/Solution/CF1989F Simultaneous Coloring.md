---
dg-publish: true
source: Codeforces
contest: Educational Codeforces Round 167
cid: "1989"
problem: F
name: Simultaneous Coloring
tags:
  - parallel-binary-search
  - connectivity
lead: 整体二分，对每条边找出其两端被缩成一点的的最早时刻
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

$
\newcommand{\red}{\text{R}}
\newcommand{\blue}{\text{B}}
\newcommand{\compr}{\mathop{\rm Compr}}
$给出一个 $n\times m$ 的矩阵，有两种染色方式：
1. 将 $n$ 列中的一行涂上红色 $\red$
2. 将 $m$ 列中的一列涂上蓝色 $\blue$

每次操作包含 $k$ 次染色。如果 $k=1$，即每次只给矩阵一行或一列染色，则代价为 $0$。如果 $k>1$，则一个格子有可能被两次染色，其在操作后的颜色可以是两种中的任意一种，代价为 $k^2$。

现在有 $q$ 个限制条件，第 $i$ 个限制条件为 $x_i, y_i, c_i$，代表 $(x_i,y_i)$ 处的颜色是 $c_i$。

矩阵初始为空，对每一个 $1\le i\le q$，求使其满足前 $i$ 个限制条件的最小操作代价。

数据范围 $1\le n,m\le 10^5$，$q\le 10^5$。

|          记号 | 含义                                                         |
| ----------: | :--------------------------------------------------------- |
|   $G=(V,E)$ | 有向图 $G$，其中 $V$ 是点集，$E$ 是边集                                 |
|      $f(G)$ | 将题目中的限制转化为图的形式后图 $G$ 对应的题目中最小代价，其为每一个大小非 $1$ 的强连通分量大小的平方和。 |
|    $E_{<m}$ | $E_{<m}=\{(u_i,v_i)\mid i<m\}$                             |
| $E_{\ge m}$ | $E_{\ge m}=\{(u_i,v_i)\mid i\ge m\}$                       |
| $\compr(G)$ | $G$ 将强连通分量缩为一点后的点集，也保留其映射                                  |

### 题解

#### 问题转化

因为只给一行或一列染色的代价是 $0$，所以我们可以假设每行每列都被染色。同时，如果有一行被两次染色，那么可以直接将第一次染色给去除，因此我们进一步假设每行每列都被染色恰好一次。

我们考虑 $x,y,c$ 的限制条件的实际意义。如果 $(x,y)$ 是红色，则第 $x$ 行不可能第 $y$ 列之前被染色。同样，如果 $(x,y)$ 是蓝色，则第 $y$ 列不可能在第 $x$ 行之前被染色。

我们对第 $i$ 行建一个点 $r_i$，对第 $i$ 列建一个点 $c_i$，用边来表示染色的先后顺序，则 $x,y,\text{R}$ 的限制就相当于从 $r_x$ 向 $c_y$ 连一条有向边，$x,y,\blue$ 的限制就相当于从 $c_y$ 向 $r_x$ 连一条有向边。显然，如果要满足所有的限制条件，图里每一个强连通分量都必须在同一次操作中染色。又因为同时进行 $k$ 次染色的代价是 $k^2$，所以代价最小的方案就是对每个强连通分量按拓扑序依次进行染色。

这样，我们就把问题转化为了如下形式：

给出 $m$ 条单向边 $(u_1,v_1),\dots,(u_m,v_m)$。令有向图 $G_i=(V,E_i)$，其中 $E_0=\emptyset$，$E_i=E_{i-1}\cup(u_i,v_i)$，令 $f(G)$ 代表 $G$ 中大小非 $1$ 的强连通分量大小的平方和，对每一个 $1\le i\le m$，求 $f(G_i)$。

#### 整体二分

接下来，我们使用离线后的整体二分来解决这个问题。

不妨令当前要对 $G_l,\dots,G_r$ 求强连通分量。把区间按中点 $m$ 分成 $[l,m)$ 和 $[m,r)$ 两部分，对 $G_l,\dots,G_{m-1}$ 和 $G_m,\dots,G_{r-1}$ 分别考虑哪些边会对其连通分量产生影响。令该区间中边集为 $E$，将其根据原始编号分成两半 $E_{<m}$ 和 $E_{\ge m}$。显然 $E_{\ge m}$ 不会对 $G_l,\dots,G_{m-1}$ 产生影响，因此我们只需要考虑 $E_{<m}$ 的影响。

我们对 $E_{<m}$ 做一次强连通分量分解，这样可以把边分为两类，第一类是两端在同一个 SCC 里，令其为 $E_1$，第二类是两端不在同一个强连通分量里，令其为 $E_2$。因为 $E_2$ 中的边不可能在 $m$ 时刻以前形成强连通分量，因此 $E_2$ 对 $G_l,\dots,G_{m-1}$ 没有贡献。而 $E_1$ 在 $m$ 时刻以前就在一个强连通分量内部，因此只要用并查集维护 $G_i$ 根据强连通分量的缩点情况就足以处理 $E_1$ 对 $G_m,\dots,G_{r-1}$ 的贡献。不妨令 $G_i$ 根据强连通分量缩点后的点集为 $V'_i=\compr(G_i)$，则我们如下设计算法：

给出 $V'_{l-1}=\compr(G_{l-1})$，待求解的区间 $[l,r)$ 以及对 $G_l,\dots,G_r$ 有贡献的边集 $E$，执行以下步骤：
1. 如果 $r-l=1$，将 $E$ 映射到 $V'_{l-1}$ 后做强连通分量分解，得到 $V'_l$，$f(G_l)$ 并返回
2. 否则，根据区间中点 $m$ 将 $[l,r)$ 分成两部分 $[l,m)$ 和 $[m,r)$，将 $E$ 根据原始编号分为 $E_{<m}$ 和 $E_{\ge m}$
3. 将 $E_{<m}$ 映射到 $V'_{l-1}$ 后做强连通分量分解，根据边的两端是否在同一强连通分量进一步将 $E_{<m}$ 分为两类 $E_1,E_2$
4. 将 $V'_{l-1}$，$[l,m)$ 和 $E_1$ 递归求解，保留 $V'_{m-1}$
5. 将 $V'_{m-1}$，$[m,r)$ 和 $E_2\cup E_{\ge m}$ 递归求解，保留并返回 $V'_{r-1}$

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
struct edge {
  int u, v, i;
};
struct dsu {
  vector<int> f;
  vector<ll> cnt;  // cnt: |scc|
  ll sum;          // sum of |scc > 1|^2
  dsu(int n) : f(n + 1), cnt(n + 1, 1), sum(0) { iota(f.begin(), f.end(), 0); }
  int ff(int u) { return f[u] == u ? u : f[u] = ff(f[u]); }
  // merge two sccs
  void comb(int u, int v) {
    u = ff(u), v = ff(v);
    if (u == v) return;
    sum -= cnt[u] > 1 ? cnt[u] * cnt[u] : 0; // only |scc|>1 counts
    sum -= cnt[v] > 1 ? cnt[v] * cnt[v] : 0;
    f[u] = v;
    cnt[v] += cnt[u];
    sum += cnt[v] > 1 ? cnt[v] * cnt[v] : 0;
  }
};
const int maxn = 5e5;
// return sccs for given edges
vector<vector<int>> scc_decomp(vector<pair<int, int>> ed) {
  static vector<vector<int>> e(maxn);
  static vector<int> dfn(maxn, 0), low(maxn, 0);
  vector<vector<int>> res;
  for (auto [u, v] : ed) e[u].push_back(v);
  vector<int> stk;
  static vector<bool> ins(maxn);
  int k = 0;
  auto dfs = [&](auto &&self, int u) -> void {
    stk.push_back(u), ins[u] = true, dfn[u] = low[u] = ++k;
    for (auto v : e[u])
      if (!dfn[v]) {
        self(self, v);
        low[u] = min(low[u], low[v]);
      } else if (ins[v]) {
        low[u] = min(low[u], dfn[v]);
      }
    if (dfn[u] == low[u]) {
      res.push_back({});
      int v;
      do {
        v = stk.back(), res.back().push_back(v);
        stk.pop_back(), ins[v] = false;
      } while (v != u);
    }
  };
  for (auto [u, v] : ed)
    if (!dfn[u]) dfs(dfs, u);
  // clear
  for (auto [u, v] : ed) {
    e[u].clear();
    dfn[u] = dfn[v] = low[u] = low[v] = 0;
  }
  return res;
}
vector<ll> solve(vector<edge> e, int n) {
  vector<ll> res(e.size());
  dsu d(n);
  auto toedge = [&](const vector<edge> &e) {
    vector<pair<int, int>> res;
    for (auto [u, v, i] : e) res.push_back({d.ff(u), d.ff(v)});
    return res;
  };
  vector<int> f(n + 1);
  auto divide = [&](auto &&self, int l, int r, vector<edge> &a) -> void {
    if (r - l == 1) {
      auto sccs = scc_decomp(toedge(a));
      for (auto scc : sccs)
        for (auto v : scc) d.comb(v, scc.back());
      res[l] = d.sum;
      return;
    }
    int m = (l + r) / 2;
    vector<edge> b, p, q;
    for (auto e : a) (e.i < m ? b : q).push_back(e);
    auto sccs = scc_decomp(toedge(b));
    for (auto scc : sccs)
      for (auto v : scc) f[v] = scc.back();
    for (auto e : b) (f[d.ff(e.u)] == f[d.ff(e.v)] ? p : q).push_back(e);
    a.clear(), b.clear();
    self(self, l, m, p), self(self, m, r, q);
  };
  divide(divide, 0, e.size(), e);
  return res;
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, m, q;
  cin >> n >> m >> q;
  vector<edge> e(q);
  for (int i = 0; i < q; i++) {
    string s;
    cin >> e[i].u >> e[i].v >> s, e[i].v += n, e[i].i = i;
    if (s[0] == 'B') swap(e[i].u, e[i].v);
  }
  auto ans = solve(e, n + m);
  for (auto &v : ans) cout << v << el;
  return 0;
}
```