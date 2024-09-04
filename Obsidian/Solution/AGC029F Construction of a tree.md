---
dg-publish: true
source: AtCoder
contest: AtCoder Grand Contest 029
cid: agc029
problem: F
name: Construction of a tree
tags:
  - flow
  - matching
lead: 将原图转化为二分图，再在二分图匹配意义下求方案
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

$
\newcommand{\G}{\mathcal{G}}
\newcommand{\U}{\mathcal{U}}
\newcommand{V}{\mathcal{V}}
\newcommand{E}{\mathcal{E}}
\newcommand{\el}{\mathcal{E}_L}
\newcommand{\er}{\mathcal{E}_R}
\newcommand{\res}{\mathop{\rm R}}
$给出 $n$ 个点和 $n-1$ 个集合，第 $i$ 个集合为 $E_i$，求是否可能从每个 $E_i$ 中选出一条边 $(u_i,v_i)$（$u_i,v_i\in E_i$）使这 $n-1$ 条边构成一颗生成树，并给出方案。

数据范围 $n\le 10^5$，$\left|E_i\right|\le 2\cdot 10^5$。

|                           记号 | 含义                                                                  |
| ---------------------------: | :------------------------------------------------------------------ |
|                          $V$ | 图中的点集，编号分别为 $1,\dots,n$                                             |
|                        $E_i$ | 第 $i$ 个集合，要求生成树的第 $i$ 条边 $(u_i,v_i)$ 从 $E_i$ 中选出，即 $u_i,v_i\in E_i$ |
|              $\G=(\U,\V,\E)$ | 题解中构造的二分图                                                           |
|                         $\U$ | 二分图左侧的点集，其对应 $n-1$ 个集合 $E_i$                                        |
|                         $\V$ | 二分图右侧的点集，其对应 $n$ 个点 $1,\cdots,n$，即 $\V=V$                           |
|                         $\E$ | 二分图的边集，其对应 $E_i$ 与 $V$ 中的点的包含关系，即 $\E=\{(i,j)\mid j\in E_i\}$       |
| $\res(\G,M)=(\U,\V,\el,\er)$ | 匹配 $M$ 对应的从右侧出发的交错网络                                                |
|                        $\el$ | 交错网络中左侧指向右侧的边集，即 $\el=M$                                            |
|                        $\er$ | 交错网络中右侧指向左侧的边集，即 $\er=E\setminus M$                                 |

### 题解

建二分图 $\G=(\U,\V,\E)$，其中
- 左侧的点集 $\U$ 为 $n-1$ 个集合 $E_i$
- 右侧的点集 $\V$ 为图中的 $n$ 个点，即 $\V=V$
- 边集 $\E$ 为集合 $E_i$ 与图中的点的包含关系，即 $\E=\{(i,j)\mid j\in E_i\}$。

不妨假设存在满足要求的生成树，令任意一个点 $r$ 为根，则每个点 $u$ 和其指向父亲的边 $e$ 一一对应。因此，在删除任意一个右侧的点 $r$ 后，二分图 $\G$ 都存在一个大小为 $n-1$ 的完美匹配。

我们考虑这个匹配中的 **交错路**，即从非匹配点 $r$ 出发，由非匹配边与匹配边交错而成的路径。我们发现，生成树根 $r$ 到任意一个点 $u$ 的路径都对应了一条交错路，因此这个匹配中 $r$ 到任何一个 $\V$ 中的点都通过交错路可达。

因为讨论中非匹配点只有 $r$，因此交错路中匹配边一定是左侧指向右侧，非匹配边一定是右侧指向左侧。因此，对于一个除 $r$ 后的完美匹配 $M$，我们定义 **交错网络** 为从右侧出发的交错路构成的图 $\res(\G,M)=(\U,\V,\el,\er)$，其中
- $\el$ 为交错路中左侧指向右侧的边集，即 $\el=M$
- $\er$ 为交错路中右侧指向左侧的边集，即 $\er=E\setminus M$

如果熟悉最大流，可以看出这里定义的交错网络就是残余网络的反图。

这样，我们可以将上面的命题重写为以下形式：

如果存在满足要求的生成树，则在删去任意一个点 $r$ 后，$\G$ 中都存在一个完美匹配 $M$，且在交错网络 $\res(\G,M)$ 中 $r$ 到每个点都可达。

下面，我们来证明其逆命题，即如果 $\G$ 删除任意一个点 $r$ 后存在一个完美匹配 $M$ 且交错网络 $\res(\G,M)$ 中 $r$ 到每个点都可达，则存在满足要求的生成树。

可以通过构造证明。因为 $r$ 到每个点都可达，所以任意一棵从 $r$ 出发的 DFS 树都包含了所有点。因为所有左侧的点都只有一条出边，所以其入度出度都为 $1$，其直接构成一个生成树方案，即对于每一个集合 $E_i$，选择其在 DFS 树上唯一的父亲和唯一的儿子作为一条边。

### 注意事项

- 实践中并不需要将匹配中的边从 $\er$ 中删去

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;

// Details of maximum matching is omitted because it is too ugly.
vector<pair<int,int>> match::max_match(vector<pair<int,int>> &edge){
  // TODO: Rewrite maximum matching
  // For current version, please refer to:
  // https://github.com/Kilo-5723/CP-Template/blob/master/graph/maxflow_old.h
  // https://github.com/Kilo-5723/CP-Template/blob/master/graph/match.h
}

optional<vector<pair<int, int>>> solve(vector<pair<int, int>> edge,
                                       vector<pair<int, int>> match) {
  int n = match.size() + 2;
  vector<vector<int>> rtl(n);  // edge from right to left
  vector<int> ltr(n);          // edge from left to right
  for (auto [u, v] : match) ltr[u] = v;
  for (auto [u, v] : edge) rtl[v].push_back(u);
  // no need to exclude matching in practice
  int s;
  vector<bool> vis(n);
  for (int i = 1; i < n - 1; i++) vis[ltr[i]] = true;
  for (int i = 1; i < n; i++)
    if (!vis[i]) s = i;  // unmatched vertex
  vector<int> fol(n);    // father of a left node (represetning an edge)
  auto dfs = [&](auto &&self, int u) -> void {
    for (auto l : rtl[u])
      if (!fol[l]) {
        fol[l] = u;
        self(self, ltr[l]);
      }
  };
  dfs(dfs, s);
  for (int i = 1; i < n - 1; i++)
    if (!fol[i]) return nullopt;  // unreachable vertex
  vector<pair<int, int>> res;
  for (int i = 1; i < n - 1; i++) res.push_back({fol[i], ltr[i]});
  return res;
}

int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n;
  cin >> n;
  vector<pair<int, int>> edge;
  for (int i = 1; i < n; i++) {
    int t;
    cin >> t;
    while (t--) {
      int j;
      cin >> j;
      edge.push_back({i, j});
    }
  }
  auto mch = match::max_match(edge);  // maximum matching
  if (mch.size() < n - 1) {           // no perfect match
    cout << -1 << el;
    return 0;
  }
  auto ans = solve(edge, mch);  // find dfs tree
  if (!ans) {                   // r not reachable to every vertex
    cout << -1 << el;
    return 0;
  }
  for (auto [u, v] : *ans) cout << u << ' ' << v << el;
  return 0;
}
```

- [ ] #task Add Matching Template