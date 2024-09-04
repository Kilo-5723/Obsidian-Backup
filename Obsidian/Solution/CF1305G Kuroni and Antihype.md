---
dg-publish: true
source: Codeforces
contest: Ozon Tech Challenge 2020 (Div. 1 + Div. 2)
cid: "1305"
problem: G
name: Kuroni and Antihype
tags:
  - graph
  - bitmask
  - reweighting
lead: 最小树形图，边权重构后转化为最小生成树
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

$
\newcommand{\gain}{\mathop{\rm gain}}
\newcommand{\and}{\mathop{\&}}
\newcommand{\ack}{\mathop{\rm A}}
$给出 $n$ 个权重为 $a_1,\dots,a_n$ 的结点，将这些结点以任意顺序加入一个初始为空的集合 $S$，可以使用的操作为：
1. 直接将结点 $u$ 加入 $S$，得分为 $0$
2. 用 $\and$ 表示 [二进制与运算](https://en.wikipedia.org/wiki/Bitwise_operation)。在 $S$ 中选择一个满足 $a_u\and a_v=0$ 的父亲 $v$，让 $u$ 被 $v$ 扩展，成为 $v$ 的孩子并加入 $S$，得分为 $v$

求所有方案中最高的总得分。

点的数量 $n\le 2\cdot 10^5$，值域 $a_1,\dots,a_n\in[0,A]$ 满足 $A\le 2\cdot 10^5$。

|            记号 | 含义                           |
| ------------: | :--------------------------- |
|     $a\and b$ | 二进制与运算                       |
|           $A$ | $a_1,\dots,a_n$ 的最大值         |
|     $G=(V,E)$ | 有向图或无向图，其中 $V$ 为点集，$E$ 为边集   |
|     $(u,v,w)$ | 从 $u$ 到 $v$，权值为 $w$ 的有向边或无向边 |
|   $\ack(m,n)$ | 阿克曼函数                        |
| $\alpha(m,n)$ | 反阿克曼函数                       |

### 题解

对于第一种操作，我们可以假设 $S$ 初始包含一个权重 $a_0=0$ 的点，这样可以将第一种操作归约为以 $0$ 为父亲的扩展操作，最终的结果也变为了一棵以 $0$ 为根的树。

首先，我们考虑同一个值多次出现的情况。如果在加入 $u$ 时 $S$ 中已经存在权重相同的结点 $v$，那么将加入 $u$ 的操作延后不会影响最高得分，因为可以将所有以 $u$ 为父亲的扩展操作改为以 $v$ 为父亲的，得分相同的扩展操作。

因此，我们规定将扩展操作分为两个阶段，第一阶段将所有不同的值加入 $S$ 恰好一次，第二阶段将重复的值也全部加入 $S$。我们考虑第二阶段的最高得分，因为第二阶段中的结点可以任意选择所有值作为父亲，所以值为 $x$ 的点被加入时的最高得分即为 $\max \{y\mid x\and y=0,y\in \{a_0,\dots,a_n\}\}$，可以用子集枚举在 $O(n+3^{\log_2 A})$，即 $O(n+A^{\log_2 3})$ 的时间内求出第二阶段的最高得分。

#### 最小树形图

接下来，我们处理第一阶段的最高得分。不妨将问题规约为 $a_i$ 中没有重复元素的形式，根据最初的转化，我们要通过扩展操作构造一棵以 $0$ 为根的树并使总得分最高。如果将扩展操作形式化建边，我们可以将问题转化为如下形式：

给出一个有向图 $G=(V,E)$，其中：
- 点集 $V=\{0,\dots,n\}$，即给出的序列加上 $S$ 的初始元素
- 边集 $E=\{(u,v,a_u)\mid a_u\and a_v=0, u,v\in V\}$，即所有合法的扩展操作

求 $G$ 边权和最大的生成树，其以 $0$ 为根并且 $0$ 到所有点都可达。

注意到该问题与 **最小树形图** 等价。然而，即使直接求解最小树形图的时间复杂度为 $O(|E|+|V|\log|V|)$，因为其常数过高且实现过于复杂，我们还是需要将问题转化为更简单的形式。

观察图 $G$，我们发现如果将每个点 $u$ 所有入边 $(v,u,a_v)$ 边权增加 $a_u$，则对于任意两点 $u,v$ 之间的边，其双向的边权都等于 $a_u+a_v$，变成了无向图的形式。

考虑这样操作会如何影响生成树的值。因为除 $0$ 以外的每个点都会被扩展恰好一次，因此将 $u$ 的所有入边边权增加 $a_u$ 会导致所有生成树的值恰好增加 $a_u$。因此，$G$ 的最大树形图等价于 $E=\{(u,v,a_u+a_v)\mid a_u\and a_v=0, u,v\in V\}$ 的无向图最大生成树，其边权之和恰好相差 $\sum a_i$。

#### 最小生成树

然而，这样转化后图中有 $O(A^{\log_2 3})$ 条边，复杂度为 $O(|E|\log|V|)$ 的 Kruskal 算法和堆优化的 Prim 算法依然无法通过本题。因此，我们考虑使用 Boruvka 算法求最小生成树，其流程如下：

1. 将每个点初始化为独立的连通分量
2. 对每个连通分量找到其边权最小的出边
3. 将每个连通分量按其最小出边合并
4. 如果 3. 中没有连通分量被合并，则结束算法，否则回到 2.

因为每轮迭代中至少有一半的连通分量被合并，所以最多迭代 $\log_2|V|$ 轮，因此在一般情况下，Boruvka 的复杂度也是 $O(|E|\log|V|)$。

然而在这道题中，因为边集是所有满足 $a_u\and a_v=0$ 的 $(u,v,a_u+a_v)$，所以可以直接用子集枚举按边权递减的顺序访问所有出边，只要对每个点维护未使用过的最大出边就可以保证每条边在 $\log_2|V|$ 轮迭代中只被遍历一次。因此，第 2 步的总复杂度为 $O(|E|+|V|\log|V|)$。考虑第 3 步的复杂度，可以发现其为总共 $|E|$ 次操作，大小为 $|V|$ 的并查集，其复杂度为 $O(|E|\cdot \alpha(|E|,|V|))$。因此，该算法的整体复杂度为 $O(|E|\cdot\alpha(|E|,|V|)+|V|\log|V|)=O(A^{\log_2 3}\cdot\alpha(A^{\log_2 3},n)+n\log n)$。

然而值得注意的是，易证阿克曼函数 $\ack(a,b)\ge a\cdot b$，因此对于反阿克曼函数
$$\alpha(m,n)=\min\{i\ge 1:\ack(i,\lfloor{m/n}\rfloor)\ge\log_2 n\}$$
有 $\alpha(m,n)\le\lceil\log_2 n/\max\{1,\lfloor m/n \rfloor\}\rceil$，故 $m\alpha(m,n)=O(m+n\log n)$。因此，算法的整体复杂度可以写为 $O(A^{\log_2 3}+n\log n)$。当然，如果在合并时暴力地将较小的连通块并入较大的连通块，我们很容易就能得到相同复杂度的算法。

注意这里的 $n$ 是对序列 $a$ 去重后剩余的元素数，因此其不超过 $A$。在考虑重复元素后，我们得到最终复杂度，其为 $O(A^{\log_2 3}+A\log A+n)=O(A^{\log_2 3}+n)$。

### 注意事项

假设中 $S$ 初始包含的 $0$ 会影响到 $a$ 序列中 $0$ 的出现次数。另外，这道题中暴力合并的表现比仅含路径压缩的并查集效果更好。

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const int k = 18;                   // maximum binary length
const int mask = (1 << k) - 1;      // bit mask
const pair<int, int> nil = {0, 0};  // edge not exist
// brute force merge of dsu
struct dsu {
  vector<int> f;
  vector<vector<int>> c;  // vertices in component
  dsu(int n) : f(n + 1), c(n + 1) {
    iota(f.begin(), f.end(), 0);
    for (int i = 0; i <= n; i++) c[i] = {i};
  }
  // merge u and v, return false if u and v are already merged
  bool merge(int u, int v) {
    u = f[u], v = f[v];
    if (u == v) return false;
    if (c[u].size() > c[v].size()) swap(u, v);
    for (auto w : c[u]) f[w] = v;
    c[v].insert(c[v].end(), c[u].begin(), c[u].end());
    return true;
  }
};
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n;
  cin >> n;
  vector<int> a(n);
  for (auto &v : a) cin >> v;
  a.push_back(0);           // suppose there's already a zero
  vector<int> cnt(1 << k);  // number of appearance of v in a
  for (auto v : a) cnt[v]++;
  sort(a.begin(), a.end());
  a.resize(unique(a.begin(), a.end()) - a.begin());
  reverse(a.begin(), a.end());
  ll ans = 0;
  // non-first appearance
  for (auto v : a) {  // inviter from large to small
    int p = mask & ~v;
    for (auto u = p; u >= 0; u = u ? u - 1 & p : -1)  // enum u & v = 0 desc
      if (cnt[u] && u < v) {
        ans += (ll)(cnt[u] - 1) * v, cnt[u] = 1;  // let v invite all u
        ans += (ll)(cnt[v] - 1) * u, cnt[v] = 1;  // largest u invite v
      }
  }
  // first appearance, boruvka
  dsu d(1 << k);
  for (auto v : a) ans -= v;
  vector<pair<int, int>> best(1 << k, nil);  // best out-edge of each component
  vector<int> cur(1 << k);  // current largest out-edge of each vertex
  for (auto v : a) cur[v] = mask & ~v;
  auto update = [&](pair<int, int> &a, pair<int, int> b) {
    if (a.first + a.second < b.first + b.second) a = b;
  };
  bool fin = false;  // finish if no update in last repeat
  while (!fin) {
    // find largest out-edge
    for (auto v : a) {
      auto &u = cur[v];
      while (u >= 0 && (!cnt[u] || d.f[u] == d.f[v]))  // skip if connected
        u = u ? u - 1 & mask & ~v : -1;                // next edge
      if (u >= 0) update(best[d.f[v]], {v, u});
    }
    fin = true;
    for (auto v : a)
      if (best[v] != nil) {
        auto [u, w] = best[v];
        if (d.merge(u, w)) ans += u + w, fin = false;
      }
    for (auto v : a) best[v] = nil;
  }
  cout << ans << el;
  return 0;
}
```