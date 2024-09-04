---
dg-publish: true
done: true
tags:
  - graph
---
# 最短路问题的图边权重构

### 问题简述

$
\newcommand{\dis}{\mathop{\rm dis}}
$给出一个带权有向图，如果要求多对结点之间的距离，一般的处理方式是对每一个起点求一次单源最短路。然而，如果这张图中存在负权边，那么只能使用 $O(|V||E|)$ 的 SPFA 求单源最短路。因此，为了使用 Dijkstra 处理单源最短路，我们希望将原图的边权重构为非负数，同时可以快速从重构后的两点距离还原出原图的两点距离。我们将这个问题正式定义如下：

给出一个有向图 $G=(V,E)$，其中每条边 $(u,v)\in E$ 有一个权重 $w(u,v)$。权重 $w(u,v)$ 可能小于 $0$，但 $G$ 中不存在负环。

我们需要构造权重 $w'(u,v)$，其满足 $w'(u,v)\ge 0$，并且可以快速计算 $\dis(u,v,w)$ 与 $\dis(u,v,w')$ 之差，其中 $\dis(u,v,w)$ 代表 $u, v$ 在权重 $w$ 下的距离，即最短路长度。

|            记号 | 含义                                                                         |
| ------------: | :------------------------------------------------------------------------- |
|     $G=(V,E)$ | 有向图 $G$，其中 $V$ 为点集，$E$ 为边集                                                 |
|      $w(u,v)$ | 权重函数，$w(u,v)$ 代表边 $(u,v)$ 上的边权，即长度                                         |
| $\dis(u,v,w)$ | $u$ 和 $v$ 在权重函数 $w$ 下的距离，即最短路长度。特殊地，如果 $u, v$ 不可达，则记为 $\dis(u,v,w)=\infty$ |
|        $h(u)$ | 结点 $u$ 的高度，定义为 $\dis(s,u,w)$，其中 $s$ 是给定的源点                                 |

### 边权重构

首先，我们引入 **高度** 的概念。考虑单源最短路的简单情形，给出源点 $s$，假设 $s$ 到每个点都可达，即 $\dis(s,u,w)<\infty$，我们定义 $u$ 的高度为 $s$ 到 $u$ 的距离，即 $h(u)=\dis(s,u,w)$。通过 [松弛操作](https://en.wikipedia.org/wiki/Bellman%E2%80%93Ford_algorithm) $\dis(s,v,w)\le\dis(s,u,w)+w(u,v)$，我们可以证明图中任意一条边的边权都不低于终点与起点高度之差。

因此，如果重构后的图中没有从较高点连向较低点的边，那么图中不存在负权边。更进一步，如果重构后的图中所有点的高度都为 $0$，图中也不存在负权边。

为了将图中所有点的高度都变为 $0$，我们引入边权重构中的核心操作，将 $u$ **提高** $x$。对于图中的任意点 $u$，将 $u$ 提高 $x$ 包含以下两步：
1. 将 $u$ 的所有入边权重加 $x$，即 $w'(v,u)=w(v,u)+x$
2. 将 $u$ 的所有出边权重减 $x$，即 $w'(u,v)=w(u,v)-x$

将 $u$ 提高 $x$ 后，我们有以下结论：
1. 原图中的所有不从 $u$ 出发或结束的路径无论是否经过 $u$ 长度都不变，因此 $\dis(v_1,v_2,w)=\dis(v_1,v_2,w')\;(v_1,v_2\neq u)$
2. 原图中所有从 $u$ 出发的路径长度减少 $x$，因此 $\dis(u,v,w')=\dis(u,v,w)-x$
3. 原图中所有到 $u$ 结束的路径长度增加 $x$，因此 $\dis(v,u,w')=\dis(v,u,w)+x$
4. 点 $u$ 新的高度为 $h'(u)=\dis(s,u,w')=h(u)+x$。

上面的分析同样适用于 $x$ 为负数的情况。因此要重构图 $G$ 的边权，也就是将所有点的高度变为 $0$，我们可以将图中每个点 $u$ 提高 $-h(u)$，则我们有：
1. 重构后，每条边 $(u,v)$ 只在提高 $u$ 和 $v$ 时分别被操作一次，因此 $(u,v)$ 的边权为 $w'(u,v)=w(u,v)+h(u)-h(v)$
2. 重构后，对任意两点 $u,v$，$u$ 到 $v$ 的距离只在提高 $u$ 和 $v$ 时被影响，因此 $u$ 到 $v$ 的距离满足 $\dis(u,v,w)=\dis(u,v,w')-h(u)+h(v)$

$G$ 的边权重构需要先对原图求一次复杂度较高的带负权边单源最短路，一般使用复杂度为 $O(|V||E|)$ 的 SPFA。因此，边权重构适用于对同一个图进行多次最短路计算的情形，例如 [[#^johnson|全源最短路]] 和 [[最小费用最大流 Minimum Cost Maximum Flow|费用流]]。另外，边权重构也不局限于去除负权边的场景，如 [[#^example2|例题 2]]。

### Johnson 全源最短路 ^johnson

给出一个带负权边无负环的有向图 $G=(V,E)$，Johnson 全源最短路可以在 $O(|V||E|+|V|^2\log|V|)$ 的时间内求出所有节点两两之间的最短路长度。其做法就是先将边权重构，再在重构图上对每个点运行一次 Dijkstra 算法求出单源最短路长度。

值得注意的是，因为原图中不一定存在可以到达所有点的源点，因此需要新建源点 $s$，其到每个点的距离为 $0$ 且不存在反向边，即 $\dis(s,u,w)=0,\; \dis(u,s,w)=\infty$。显然，$s$ 不会出现在原图中任何两点之间的路径上，因此不会影响原图中的最短路长度。在实现上，只需要将所有点的初始距离设为 $0$。

SPFA 重构图的复杂度为 $O(|V||E|)$，对每个起点做一次 Dijkstra 的复杂度为 $O(|V|\cdot(|E|+|V|\log|V|))$，整体复杂度为 $O(|V||E|+|V|^2\log|V|)$。当然，一般使用优先队列优化 Dijkstra，复杂度为 $O(|V||E|\log|V|)$。[[#^code|参考代码]]

### 例题

1. [【模板】全源最短路（Johnson）](https://www.luogu.com.cn/problem/P5905)：Johnson 全源最短路模板题
2. [Dynamic Shortest Path](https://codeforces.com/contest/843/problem/D)：用边权重构限制最远点的距离，再用桶代替堆将复杂度降低 $O(\log|V|)$ [[Solution/CF843D Dynamic Shortest Path|题解]] ^example2

### 附录

#### Johnson 全源最短路参考代码 ^code

```cpp
struct graph {
  vector<vector<pair<int, ll>>> e;
  graph(int n) : e(n + 1) {}
  void adde(int u, int v, ll w) { e[u].push_back({v, w}); }
  vector<ll> h;
  // initialize h(u), return false if there exists a negative cycle
  bool init() {
    int n = e.size();
    h.assign(n, 0);
    queue<int> que;
    for (int u = 1; u < n; u++) que.push(u);
    vector<int> vis(n, 0), cnt(n, n + 1);
    while (que.size()) {
      auto u = que.front();
      que.pop();
      vis[u] = false;
      if (!cnt[u]--) return false;  // exists a negative cycle
      for (auto &[v, w] : e[u])
        if (h[v] > h[u] + w) {
          h[v] = h[u] + w;
          if (!vis[v]) que.push(v), vis[v] = 1;
        }
    }
    return true;
  }
  // single source shortest path from given sink based on h(u)
  vector<ll> query(int s) {
    int n = e.size();
    vector<ll> dis(n, inf);
    priority_queue<pair<ll, int>, vector<pair<ll, int>>,
				   greater<pair<ll, int>>>
        que;
    que.push({dis[s] = 0, s});
    while (que.size()) {
      auto [du, u] = que.top();
      que.pop();
      if (dis[u] < du) continue;
      for (auto [v, w] : e[u]) {
        auto dv = du + w + h[u] - h[v];
        if (dis[v] > dv) que.push({dis[v] = dv, v});
      }
    }
    for (int i = 0; i < n; i++) dis[i] += h[i] - h[s];
    return dis;
  }
};
```