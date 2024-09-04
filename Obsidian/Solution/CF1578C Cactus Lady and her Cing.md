---
dg-publish: true
source: Codeforces
contest: ICPC WF Moscow Invitational Contest
cid: "1578"
problem: C
name: Cactus Lady and her Cing
tags:
  - graph
  - simulation
  - dynamic-programming
lead: 大模拟，仙人掌转圆方树后树形 DP 并输出方案
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

$
\newcommand{\go}{\mathop{\rm straight}}
\newcommand{\turn}{\mathop{\rm turn}}
\newcommand{pair}[2]{\langle{#1},{#2}\rangle}
$给出一个仙人掌，要求把点和边都嵌入到一个 $2\times\infty$ 的网格图中。

### 题解

**前置知识：**[[圆方树 Block-Cut Tree|圆方树]]，[[换根DP Rerooting DP|换根 DP]]

|                         记号 | 含义                                                                                                       |
| -------------------------: | :------------------------------------------------------------------------------------------------------- |
|                   $y_\min$ | 嵌入方案的最小 $y$ 坐标                                                                                           |
|                   $y_\max$ | 嵌入方案的最大 $y$ 坐标                                                                                           |
|                        $P$ | 嵌入方案中一条从 $y_\min$ 到 $y_\max$ 的路径，其两端度数为 $1$                                                              |
|         $L(u)=\pair{a}{b}$ | $u$ 轻子树的表述方法，下文说明它一定是链。$a,b$ 分别是 $u$ 到链两个端点的距离，如果只有一个端点则用 $\pair{1}{a}$ 表示。特殊地，如果子树为空则用 $\pair{0}{0}$ 表示 |
|   $\go(u)=\pair{y_0}{y_1}$ | 如果 $u$ 在父亲右侧，其子树在两行最小 $y$ 坐标的最大值，其中 $y_0$ 是与 $u$ 同行的最小 $y$ 坐标，$y_1$ 是与 $u$ 异行的最小 $y$ 坐标                  |
| $\turn(u)=\pair{y_0}{y_1}$ | 如果 $u$ 在父亲上侧或下侧，其子树在两行最小 $y$ 坐标的最大值，$y_0$ 和 $y_1$ 的定义同上                                                  |

 #### 树

我们先讨论问题的简单情形，即如何把一棵树嵌入一个 $2\times\infty$ 的网格图。为了方便作图，我们令纵轴为 $x$，横轴为 $y$，$y$ 坐标从左到右递增。

对于任何嵌入，一定存在一条从 $y_\min$ 到 $y_\max$ 的路径，我们用 $P$ 表示这条路径。因为网格图宽度为 $2$，所以对 $P$ 上任意前后两点 $u,v$，$v$ 只可能在 $u$ 的上下或者右侧。另外，如果 $P$ 的某一端点度数大于 $1$，则至少有一边是链，可以将链放到原端点另一侧以构造出端点度数为 $1$ 的放置方案，因此我们只讨论 $P$ 的端点度数至多为 $1$ 的情况。下面用大写字母表示 $P$ 上的点，小写字母表示 $P$ 外的点，`| =` 表示 $P$ 上的边，`: -` 表示 $P$ 外的边。可以发现：
1. 并不总能构造出一条笔直的路径
	```
	'          a - b - c   d - H===I===J===K===L - h - i
	'              :           |               |
	'  A===B===C===D===E===F===G - e - f - g   M===N===O
	```
2. $P$ 不总是直径
	```
	'          a - b - c   G===H===I===J===K===L===M===N===O
	'              :       |
	'  A===B===C===D===E===F - d - e - f - g - h - i - j
	```

因此，很难直接找出 $P$ 上的点，但如果令 $P$ 的一端为根，那么 $P$ 一定是根到某个叶子的链。所以，我们考虑用换根 DP 来确定每个点是 $P$ 的端点的可能性。可以观察到 $P$ 将网格图划分成了多个直链区域，如果将 $P$ 移出网格图，则剩下的部分都是可以简单处理的链。因此，对于结点 $u$，我们主要关注如果 $u$ 在 $P$ 上，那么 $u$ 在 $P$ 上的后继 $v$ 是 $u$ 的哪个孩子，$v$ 在 $u$ 的哪侧，以及 $v$ 的子树如何嵌入网格图。

我们称 $v$ 为 $u$ 的 **重子结点**，其子树为 **重子树**，称 $u$ 的其他子结点为 **轻子结点**，其子树为 **轻子树**。根据上文，所有的轻子树都是直链。我们用 `?` 表示 $u$ 子树以外的部分，`#` 表示 $u$ 的重子树，`;` 表示轻子树。下面，我们通过 $u$ 和 $v$ 的位置关系分类讨论：
1. $u$ 是叶节点，$v$ 不存在
	```
	'  ?===U  '  ?
	'         '  |
	'         '  U
	```
	$u$ 没有任何子树需要嵌入，$u$ 的子树占据的空间就是 $u$ 本身。

2. $v$ 在 $u$ 的右侧
	```
	'  ?===U===V===#  '  ?===U===V===#===#===#
	'          :      '  |   :           :
	'  # - # - # - #  '  ?   ; - ; - ;   # - #
	```
	因为所有轻子树都是直链，我们只关心 $v$ 的子树在两行所占据的最小 $y$ 坐标。我们令 $y_0$ 为 $v$ 同行的最小 $y$ 坐标，$y_1$ 为异行的最小 $y$ 坐标，并使用以 $v$ 为原点的相对坐标。因为 $v$ 在 $P$ 上，所以 $v$ 的子树不可能放到 $v$ 同行以左的点，$y_0=0$。而对于另一行，我们则希望 $v$ 的子树在这行的最小 $y$ 坐标越大越好。我们将最优的嵌入方案记为 $\go(v)=\pair{y_0}{y_1}$，其中 $y_0=0$。

3. $v$ 在 $u$ 的另一行
	```
	'  ?===?===U   #===#  '  ?===U - ? - ?  '  ?===U - ? - ?
	'  :       |   |   :  '      |          '  :   |
	'  ?   ; - V===#   #  '  ; - V===#===#  '  ?   V===#===#
	```
	与情况 2 不同，此时 $y_0(v)$ 不一定为 $0$，但依然可以证明一个偏序优于其他所有方案的最优方案存在。如果 $v$ 有轻子结点，则其轻子树一定是一条链。$v$ 至多有两个子结点，如果两者子树都不是链则 $v$ 不可能嵌入网格图，而如果两者都是链则一定可以贪心将短的一条向左放。因此，在方案最优的前提下 $v$ 的重子树唯一，我们依然想让 $v$ 的子树在另一行的最小 $y$ 坐标最大。我们将最优的嵌入方案记为 $\turn(v)=\pair{y_0}{y_1}$，其中 $y_0$ 为 $v$ 轻子树的大小乘以 $-1$。

#### 仙人掌

接下来，我们讨论仙人掌的情况。对网格图进行 0/1 染色可以证明奇环无法嵌入网格图，因此我们只讨论环长为偶数的情况。可以发现：
1. 环一定占据网格图中连续的 $2\times k$ 个点
	```
	'  A - B - C - D   I - J - L
	'      :       :   :   :
	'      G - F - E - H - K
	```
2. 一个环至少会占据两个完整的列，因此 $P$ 一定经过每一个环
	```
	'  A===B===C===D   I - J===L
	'      :       |   :   |
	'      G - F - E===H===K
	```

因此，我们对每个环建一个点 $c$，并讨论如何将环嵌入网格图中。我们称环上度数为 $3$ 的点为 **关键点**，称环上度数为 $2$ 的点为 **非关键点**。因为只有关键点会因为其子树使环的嵌入非法，所以我们只关心如何将关键点嵌入子图。因此，我们将 $c$ 与所有关键点连边。我们用 `( . )` 代表环上的边以和其他边区分，用 `X` 代表 $c$，非关键点也用 `.` 表示。可以发现：
1. 当环的度数为 $0$ 时，可以直接将环嵌入网格图中。
	```
	'   . . .  '  . . . . .
	'  (  X  ) ' (    X    )
	'   . . .  '  . . . . .
	```
2. 当环的度数为 $1$ 时，环有两种翻转后等价的嵌入方式
	```
	'  ?===A . .   '  ?===A . . . .
	'     ( \X  )  '     ( \==X    )
	'      . . .   '      . . . . .
	```
3. 当环的度数为 $2$ 时，有：
	- 如果环长为 $4$，则无论邻点是否相邻，有至多两种放置方法
		```
		'  ?===A . B===#  '  ?===A . .   '  ?===A . .
		'     ( \X/ )     '     ( \X  )  '     ( \X\ )
		'      . . .      '  # - B . .   '      . . B===#
		```
	- 如果环长大于 $4$，则当两个邻点距离为 $1, k-1, k$ 时有至多一种合法的嵌入方式，否则没有嵌入方式
		```
		'  ?===A . . . .   '  ?===A . . . B===#  '  ?===A . . . .
		'     ( \==X    )  '     ( \==X==/ )     '     ( \==X==\ )
		'  # - B . . . .   '      . . . . .      '      . . . . B===#
		```
4. 当环的度数不小于 $3$ 时和度数为 $2$ 的情形类似，如果环长为 $4$ 则有至多两种嵌入方式，否则有至多一种嵌入方式

#### 动态规划

接下来，我们将环缩成点后加入树形 DP，处理将每个点的子树嵌入 $2\times \infty$ 的网格图中的方案。因为环上的点不可能和 $P$ 上其他点共属一列，我们将环 $c$ 相关的位置关系都定义为直行 $\go(c)$。

对于点 $u$，我们需要讨论 $u$ 和其在 $P$ 上的前驱 $f$ 及后继 $v$ 的位置关系，其中 $f$ 即为 $u$ 在树上的父亲，$v$ 是 $u$ 的某个子结点，位置关系则有直行 $\go$ 和变行 $\turn$ 两种可能。不妨枚举 $v$ 是 $u$ 的哪个子结点，并根据 $f,u,v$ 三者之间的位置关系分类讨论：

1. $\go(u),\go(v)$
	```
	'  ?===?===?===U===V===c===c===c===c
	'              :               :
	'      a - a - x - b - b - b   c - c
	```
	令 $\pair{0}{y_1}=\go(v)$。这种情况下 $u$ 至多有一个轻子树 $L_u$，其是一条至多有两个叶结点的链。我们用 $L_u=\pair{a}{b}$ ($a\le b$) 表示这条链，其中 $a,b$ 分别是 $u$ 到链上两个叶结点的距离，如果只有一个叶结点，则用 $L_u=\pair{1}{a}$ 表示，没有轻子树则用 $L_u=\pair{0}{0}$ 表示。以图中的链为例，其可以用 $\pair{3}{4}$ 表示。我们有：
	1. $a=b=0$，$\go(u)=\pair{0}{y_1+1}$
	2. $0<b\le y_1+1$，$\go(u)=\pair{0}{1-a}$
	3. $a\le y_1+1<b$，$\go(u)=\pair{0}{1-b}$
	4. $a>\max\{0,y_1+1\}$，$\go(u)$ 不存在
 1. $\go(u),\turn(v)$
	```
	'  ?===?===?===U - a - a   c===c
	'              |           |
	'      b - b - V===c===c===c - c
	```
	令 $\pair{y_0}{y_1}=\turn(v)$，此时 $u$ 至多有一个轻子树 $L_u=\pair{x}{a}$。以图中的链为例，其可以用 $\pair{1}{2}$ 表示。我们有：
	1. $x\le 1$ 且 $a<y_1$，$\go(u)=\pair{0}{y_0}$
	2. $x>1$ 或 $a\ge y_1$，$\go(u)$ 不存在
3. $\turn(u),\go(v)$
	```
	'  ?===?===? - ?   b   b===b===b
	'  :       |       :   |   :
	'  ?   a - U===V===b===b   b - b
	```
	令 $\pair{0}{y_1}=\go(v)$，此时 $u$ 至多有一个轻子树 $L_u=\pair{x}{a}$，$\turn(u)=\pair{-a}{y_1+1}$。
4. $\go(c),\go(v)$
	```
	'  ?   b - b . . . a - a   c===c===c
	'  |      ( /==X==\ )      |       :
	'  ?===?===? . . . V===c===c - c   c
	```
	我们将环对应节点的位置与 $\go(u)$ 一样，视为在父亲的右边一格。可以发现，如果上一步是环，那么无论右边哪个子结点是 $c$ 的重子结点，其转移方式都是相同的。令 $\pair{0}{y_1}=\go(v)$，此时 $c$ 至多有两个轻子树，我们分别用 $\pair{x}{a}$ 和 $\pair{y}{b}$ 表示 $v$ 同侧和异侧的链，则：
	1. $a\le y_1$ 或都不存在，$\go(c)=\pair{0}{-b}$
	2. $a>y_1$，$\go(c)$ 不存在

最后，要输出方案，我们只需要以转移方程同样的形式给每个点标上坐标。因为每个点的度数都至多为 $4$，算法的复杂度是线性的。

### 注意事项

在尝试将环嵌入网格图中时，我们可以暴力枚举顺逆时针两个方向，并根据关键点找到所有左下角可能的环上坐标，并将环的子树匹配进去。可以规定如果环不为树根，则父亲一定被放到左下。

这道题的细节 **非常之多**，代码也 **非常之长**，具体实现可以看参考代码。

```cpp
#include <bits/stdc++.h>
// #define DEBUG
// #define INFO
using namespace std;
const char el = '\n';
typedef long long ll;
const int inf = 1e9;
// return the block cut tree of given graph
// if given graph is cactus, edges of square are ordered as the cycle
vector<vector<int>> block_cut(const vector<vector<int>> &e) {
  int n = e.size();
  vector<int> ord(n);  // dfs order, 1-index
  vector<int> low(n);  // lowest ord(u) reachable by at most one backward edge
  vector<int> stk;     // current vertices in dfs stack
  // modified tarjan algorithm for finding bridges
  vector<vector<int>> res(n);
  auto tarjan = [&](auto &&self, int u, auto &&id) -> void {
    ord[u] = low[u] = ++id;
    stk.push_back(u);
    for (auto v : e[u]) {
      if (ord[v]) {  // visited, it's a backward edge
        low[u] = min(low[u], ord[v]);
        continue;
      }
      self(self, v, id);               // not visited, search subtree
      low[u] = min(low[u], low[v]);    // not a backward edge
      if (low[v] != ord[u]) continue;  // not bridge or biconnected component
      if (stk.back() == v) {           // (u, v) is a bridge
        res[u].push_back(v), res[v].push_back(u);  // add (u, v) to result
        stk.pop_back();
        continue;
      }
      res.push_back({});  // a new biconnected component found
      int w;
      do {
        w = stk.back(), stk.pop_back();
        res[n].push_back(w), res[w].push_back(n);  // add (w, n) to result
      } while (w != v);
      res[n].push_back(u), res[u].push_back(n);  // add (u, n) to result
      n++;
    }
  };
  for (int i = 1; i < e.size(); i++)
    if (!ord[i]) tarjan(tarjan, i, 0);
  return res;
}
const pair<int, int> nil = {-inf, -inf};  // nil stands for no-solution
// note that max(a,b) gives the better plan among a and b
struct tree {
  vector<vector<pair<int, int>>> e;  // edge (v,w), w is id on cycle u or v
  vector<vector<int>> c;             // nodes on cycle
  vector<pair<int, int>> go, turn;   // go = straight in paper, nil if no-sol
  vector<pair<int, int>> link;       // <a,b> if subtree is link, nil otherwise
  vector<int> x, y;                  // position of each node
  // initialize by a block-cut tree, return false if fails precheck
  bool init(vector<vector<int>> tr, int n) {
    int m = tr.size();
    e.resize(m), c.resize(m), go.resize(m), turn.resize(m), link.resize(m),
        x.resize(m), y.resize(m);
#ifdef DEBUG
    cout << "construct, n = " << n << ", m = " << m << endl;
#endif
    for (int u = n + 1; u < m; u++) {
      if (tr[u].size() & 1) return false;  // odd cycle
      c[u] = tr[u];                        // store the cycle of square node
      for (int i = 0; i < tr[u].size(); i++) {
        auto v = tr[u][i];  // only link original 3-degree, now 2-degree nodes
        if (tr[v].size() == 2) e[u].push_back({v, i}), e[v].push_back({u, i});
      }
#ifdef INFO
      cout << "cycle " << u << ": ";
      for (auto v : c[u]) cout << v << ' ';
      cout << "; key nodes: ";
      for (auto [v, w] : e[u]) cout << "(" << v << ", " << w << ")" << ' ';
      cout << endl;
#endif
      if (e[u].size() > 4) return false;  // degree of cycle > 4
    }
#ifdef DEBUG
    cout << "construct, cycle fin" << endl;
#endif
    for (int u = 1; u <= n; u++)
      for (auto v : tr[u])
        if (v <= n) e[u].push_back({v, 0});
#ifdef DEBUG
    cout << "construct, vertex fin" << endl;
#endif
#ifdef INFO
    cout << "Graph:" << el;
    for (int i = 1; i < m; i++) {
      cout << i << ": ";
      for (auto [v, w] : e[i]) cout << "(" << v << ", " << w << ")" << ' ';
      if (c[i].size()) {
        cout << " ; ";
        for (auto v : c[i]) cout << v << ' ';
      }
      cout << endl;
    }
#endif
    return true;
  }
  // update link(u), return updated link(u)
  // f is father, i is id of f on the cycle of u, f = u if u is root
  pair<int, int> getlink(int u, int f) {
#ifdef DEBUG
    cout << "getlink, subtree of " << u << " from " << f << endl;
#endif
    auto &res = link[u];
    res = nil;                    // not link by default
    if (c[u].size()) return res;  // cycle cannot be on link
#ifdef DEBUG
    cout << "getlink, " << u << " is not a cycle" << endl;
#endif
    vector<int> sub;  // find subtree
    for (auto [v, w] : e[u])
      if (v != f) {
        sub.push_back(v);  // subtree of v has to be link start from u
        if (link[v].first != 1) return res;
      }
#ifdef DEBUG
    cout << "getlink, subtree of " << u << " from " << f << " found" << endl;
#endif
    switch (sub.size()) {
      case 0:  // no subtree
        res = {1, 1};
        break;
      case 1:  // one link from u
        res = {1, link[sub[0]].second + 1};
        break;
      case 2:  // two links from u
        res = {link[sub[0]].second + 1, link[sub[1]].second + 1};
        if (res.first > res.second) swap(res.first, res.second);
    }
    return res;
  }
  // update go(u), return updated go(u)
  // f is father, i is id of f on the cycle of u, f = u if u is root
  pair<int, int> getgo(int u, int f, int i) {
    auto &res = go[u];
    res = nil;
    if (!c[u].size()) {  // u is not a cycle
#ifdef DEBUG
      printf("getgo(%d), %d is not a cycle\n", u, u), fflush(stdout);
#endif
      vector<int> sub;  // subtree
      for (auto [v, w] : e[u])
        if (v != f) sub.push_back(v);
#ifdef DEBUG
      printf("subtree of %d found:", u), fflush(stdout);
      for (auto v : sub) printf(" %d", v), fflush(stdout);
      cout << endl;
#endif
      if (sub.size() == 0) return res = {0, inf};  // case of one node
      if (sub.size() > 2) return res;              // at most two subtrees
      for (auto &v : sub) {                        // enumerate heavy child
        pair<int, int> light = {0, 0};             // light subtree
        if (sub.size() == 2) light = link[sub[0] + sub[1] - v];
        if (light == nil) continue;  // other subtree not light
        if (go[v] != nil) {          // go(v)
          auto [a, b] = light;
          auto [y0, y1] = go[v];
          if (!a)
            res = max(res, {0, y1 + 1});
          else if (b <= y1 + 1)
            res = max(res, {0, 1 - a});
          else if (a <= y1 + 1)
            res = max(res, {0, 1 - b});
        }
        if (turn[v] != nil) {  // turn(v)
          auto [x, a] = light;
          auto [y0, y1] = turn[v];
          if (x <= 1 && a < y1) res = max(res, {0, y0});
        }
      }
    } else {
      auto ed = e[u];
      for (int t = 0; t < 2; t++) {     // clockwise and counter-clockwise
        reverse(ed.begin(), ed.end());  // reverse the order of cycle
        for (auto &[v, w] : ed) w = c[u].size() - 1 - w;
        i = c[u].size() - 1 - i;
        vector<int> ld = {0};  // all left-down corners, 0 in case of cycle
        if (ed.size()) {
          auto [v, w] = ed[0];  // index of an arbitrary node on cycle
          int n = c[u].size();  // length of cycle
          // possibility of v on four corners
          // ld = {w, (w + 1) % n, (w + n / 2) % n, (w + n / 2 + 1) % n};
          ld = {w,
                (w + n - 1) % n,
                (w + 1) % n,
                (w + n / 2 - 1) % n,
                (w + n / 2) % n,
                (w + n / 2 + 1) % n};
        }
#ifdef INFO
        printf("get go(%d), cycle = ", u);
        for (auto [v, w] : ed) printf("(%d, %d) ", v, w);
        puts("");
        printf("left down possible: ");
        for (auto v : ld) cout << v << ' ';
        cout << endl;
#endif
        // try matching the cycle with a left-down corner of w
        // return index of lu, ru, rd, if cannot match return nullopt
        auto get = [&](int u, int f, int i,
                       int w) -> optional<tuple<int, int, int>> {
          int n = c[u].size();
          if (f != u && i != w) return nullopt;  // rule father to be ld
          int lu = (w + n - 1) % n, ru = (w + n / 2) % n,
              rd = (w + n / 2 - 1) % n;  // position of three other points
#ifdef INFO
          cout << f << ' ' << lu << ' ' << ru << ' ' << rd << el;
          for (auto [v, w] : ed) cout << "(" << v << "," << w << ")" << ' ';
          cout << el;
#endif
          int vlu = 0, vru = 0, vrd = 0;  // id of lu, ru, rd, 0 if not exist
          for (auto [v, w] : ed) {
            if (w == lu)
              vlu = v;
            else if (w == ru)
              vru = v;
            else if (w == rd)
              vrd = v;
            else if (v != f)
              return nullopt;  // edge not matched
          }
          return make_tuple(vlu, vru, vrd);
        };
        for (auto w : ld) {
          auto opt = get(u, f, i, w);
          if (!opt) continue;
          auto [lu, ru, rd] = *opt;
#ifdef INFO
          printf("matched, %d %d %d\n", lu, ru, rd);
#endif
          // link(lu), go(ru), go(rd) must exist if lu, ru, rd exist
          if (lu && (link[lu] == nil || link[lu].first != 1) ||
              ru && go[ru] == nil || rd && go[rd] == nil)
            continue;
#ifdef INFO
          printf("still matched, %d %d %d\n", lu, ru, rd);
#endif
          int b = lu ? link[lu].second : 1;  // len = 1 if non-important
          // no heavy child
          if (!ru && !rd) res = max(res, {0, -b});
          // rd is heavy child
          if (!ru || link[ru] != nil && link[ru].first == 1) {
            int a = ru ? link[ru].second : 1;
            auto [y0, y1] = go[rd];
            if (a <= y1) res = max(res, {0, -b});
          }
          // ru is heavy child
          if (!rd || link[rd] != nil && link[rd].first == 1) {
            int a = rd ? link[rd].second : 1;
            auto [y0, y1] = go[ru];
            if (a <= y1) res = max(res, {0, -b});
          }
        }
      }
    }
    return res;
  }
  // update turn(u), return updated turn(u)
  // f is father, i is id of f on the cycle of u, f = u if u is root
  pair<int, int> getturn(int u, int f, int i) {
    auto &res = turn[u];
    res = nil;
    if (!c[u].size()) {  // u is not a cycle
      vector<int> sub;   // subtree
      for (auto [v, w] : e[u])
        if (v != f) sub.push_back(v);
      if (sub.size() > 2) return res;              // at most two subtrees
      if (sub.size() == 0) return res = {0, inf};  // case of one node
      for (auto &v : sub) {                        // enumerate heavy child
        pair<int, int> light = {0, 0};             // light subtree
        if (sub.size() == 2) light = link[sub[0] + sub[1] - v];
        if (light == nil || light.first > 1) continue;  // not link from u
        if (go[v] != nil) {                             // go(v)
          auto [x, a] = light;
          auto [y0, y1] = go[v];
          res = max(res, {-a, y1 + 1});
        }
      }
    }
    return res;
  }
  void update(int u, int f, int i) {
#ifdef DEBUG
    cout << "update, u = " << u << ", f = " << f << ", i = " << i << endl;
#endif
    link[u] = getlink(u, f);
#ifdef INFO
    printf("link(%d) = <%d, %d>\n", u, link[u].first, link[u].second),
        fflush(stdout);
#endif
    go[u] = getgo(u, f, i);
#ifdef INFO
    printf("go(%d) = <%d, %d>\n", u, go[u].first, go[u].second)
#endif
        turn[u] = getturn(u, f, i);
#ifdef INFO
    printf("turn(%d) = <%d, %d>\n", u, turn[u].first, turn[u].second);
#endif
  }
  // find position of link from u
  // op = 0: to left, op = 1: to right
  // op = 2: a - u - b, op = 3: b - u - a
  void makelink(int u, int f, int op) {
    auto res = link[u];
    vector<int> sub;  // find subtree
    for (auto [v, w] : e[u])
      if (v != f) sub.push_back(v);
    if (op < 2) {
      if (sub.size() == 1) {
        int v = sub[0];
        x[v] = x[u], y[v] = y[u] + (op ? 1 : -1), makelink(v, u, op);
      }
    } else {
      int v = 0, w = 0;
      if (sub.size() >= 1) v = sub[0];
      if (sub.size() >= 2) w = sub[1];
      // v = a, w = b
      if ((v ? link[v].second : 0) > (w ? link[w].second : 0)) swap(v, w);
      // v is left, w is right;
      if (op == 3) swap(v, w);
      if (v) x[v] = x[u], y[v] = y[u] - 1, makelink(v, u, 0);
      if (w) x[w] = x[u], y[w] = y[u] + 1, makelink(w, u, 1);
    }
  }
  // find position of u in case of go(u)
  void makego(int u, int f, int i) {
    auto res = go[u];
    if (!c[u].size()) {            // u is not a cycle
      vector<pair<int, int>> sub;  // subtree
      for (auto [v, w] : e[u])
        if (v != f) sub.push_back({v, w});
      if (sub.size() == 0) return;      // case of one node
      for (auto &[v, w] : sub) {        // enumerate heavy child
        pair<int, int> light = {0, 0};  // light subtree
        int l = 0;
        if (sub.size() == 2) light = link[l = sub[0].first + sub[1].first - v];
        if (light == nil) continue;  // other subtree not light
        if (go[v] != nil) {          // go(v)
          auto [a, b] = light;
          auto [y0, y1] = go[v];
          if (!a) {
            if (res == make_pair(0, y1 + 1)) {
              x[v] = x[u], y[v] = y[u] + 1, makego(v, u, w);
              return;
            }
          } else if (b <= y1 + 1) {
            if (res == make_pair(0, 1 - a)) {
              x[v] = x[u], y[v] = y[u] + 1, makego(v, u, w);
              if (l) x[l] = !x[u], y[l] = y[u], makelink(l, u, 2);
              return;
            }
          } else if (a <= y1 + 1) {
            if (res == make_pair(0, 1 - b)) {
              x[v] = x[u], y[v] = y[u] + 1, makego(v, u, w);
              if (l) x[l] = !x[u], y[l] = y[u], makelink(l, u, 3);
              return;
            }
          }
        }
        if (turn[v] != nil) {  // turn(v)
          auto [xx, a] = light;
          auto [y0, y1] = turn[v];
          if (xx <= 1 && a < y1) {
            if (res == make_pair(0, y0)) {
              x[v] = !x[u], y[v] = y[u], maketurn(v, u, w);
              if (l) x[l] = x[u], y[l] = y[u] + 1, makelink(l, u, 1);
              return;
            }
          }
        }
      }
    } else {
      auto ed = e[u];
      auto cc = c[u];
      for (int t = 0; t < 2; t++) {     // clockwise and counter-clockwise
        reverse(ed.begin(), ed.end());  // reverse the order of cycle
        for (auto &[v, w] : ed) w = c[u].size() - 1 - w;
        i = c[u].size() - 1 - i;
        reverse(cc.begin(), cc.end());
        vector<int> ld = {0};  // all left-down corners, 0 in case of cycle
        if (ed.size()) {
          auto [v, w] = ed[0];  // index of an arbitrary node on cycle
          int n = cc.size();    // length of cycle
          // possibility of v on four corners
          // ld = {w, (w + 1) % n, (w + n / 2) % n, (w + n / 2 + 1) % n};
          ld = {w,
                (w + n - 1) % n,
                (w + 1) % n,
                (w + n / 2 - 1) % n,
                (w + n / 2) % n,
                (w + n / 2 + 1) % n};
        }
        // try matching the cycle with a left-down corner of w
        // return index of lu, ru, rd, if cannot match return nullopt
        auto get = [&](int u, int f, int i,
                       int w) -> optional<tuple<int, int, int>> {
          int n = cc.size();
          if (f != u && i != w) return nullopt;  // rule father to be ld
          int lu = (w + n - 1) % n, ru = (w + n / 2) % n,
              rd = (w + n / 2 - 1) % n;   // position of three other points
          int vlu = 0, vru = 0, vrd = 0;  // id of lu, ru, rd, 0 if not exist
          for (auto [v, w] : ed) {
            if (w == lu)
              vlu = v;
            else if (w == ru)
              vru = v;
            else if (w == rd)
              vrd = v;
            else if (v != f)
              return nullopt;  // edge not matched
          }
          return make_tuple(vlu, vru, vrd);
        };
        for (auto w : ld) {
          auto opt = get(u, f, i, w);
          if (!opt) continue;
          auto [lu, ru, rd] = *opt;
          // link(lu), go(ru), go(rd) must exist if lu, ru, rd exist
          if (lu && (link[lu] == nil || link[lu].first != 1) ||
              ru && go[ru] == nil || rd && go[rd] == nil)
            continue;
          int b = lu ? link[lu].second : 1;  // if non-important, len = 1
          auto make_cycle = [&]() {          // mark position to cycle
            int n = cc.size();
            int x0 = x[f], y0 = y[f];
            for (int i = 0; i < n / 2; i++) {
              int v1 = cc[(w + i) % n], v2 = cc[(w + n - 1 - i) % n];
              x[v1] = x0, y[v1] = y0 + i;
              x[v2] = !x0, y[v2] = y0 + i;
            }
          };
          if (!ru && !rd && res == make_pair(0, -b)) {  // no heavy child
            make_cycle();
            if (lu) makelink(lu, u, 0);
            return;
          }
          // note that this function is not necessary for v is not cycle
          auto finde = [&](int u, int vv) {
            for (auto [v, w] : e[u])
              if (v == vv) return w;
            return -1;
          };
          if (!ru ||
              link[ru] != nil && link[ru].first == 1) {  // rd is heavy child
            int a = ru ? link[ru].second : 1;
            auto [y0, y1] = go[rd];
            if (a <= y1 && res == make_pair(0, -b)) {
              make_cycle();
              if (lu) makelink(lu, u, 0);
              if (ru) makelink(ru, u, 1);
              makego(rd, u, finde(rd, u));
              return;
            }
          }
          if (!rd ||
              link[rd] != nil && link[rd].first == 1) {  // ru is heavy child
            int a = rd ? link[rd].second : 1;
            auto [y0, y1] = go[ru];
            if (a <= y1 && res == make_pair(0, -b)) {
              make_cycle();
              if (lu) makelink(lu, u, 0);
              if (rd) makelink(rd, u, 1);
              makego(ru, u, finde(ru, u));
              return;
            }
          }
        }
      }
    }
  }
  // find position of u in case of turn(u)
  void maketurn(int u, int f, int i) {
    auto res = turn[u];
    if (!c[u].size()) {            // u is not a cycle
      vector<pair<int, int>> sub;  // subtree
      for (auto [v, w] : e[u])
        if (v != f) sub.push_back({v, w});
      if (sub.size() == 0) return;      // case of one node
      for (auto &[v, w] : sub) {        // enumerate heavy child
        pair<int, int> light = {0, 0};  // light subtree
        int l = 0;
        if (sub.size() == 2) light = link[l = sub[0].first + sub[1].first - v];
        if (light == nil || light.first > 1) continue;  // not link from u
        if (go[v] != nil) {                             // go(v)
          auto [xx, a] = light;
          auto [y0, y1] = go[v];
          if (res == make_pair(-a, y1 + 1)) {
            x[v] = x[u], y[v] = y[u] + 1, makego(v, u, w);
            if (l) x[l] = x[u], y[l] = y[u] - 1, makelink(l, u, 0);
            return;
          }
        }
      }
    }
  }
  // reroot dp
  bool reroot(int u) {
    auto init = [&](auto &&self, int u, int f, int i) -> void {
#ifdef DEBUG
      cout << "init, u = " << u << ", f = " << f << ", i = " << i << endl;
#endif
      for (auto [v, w] : e[u])
        if (v != f) self(self, v, u, w);
#ifdef DEBUG
      cout << "init, subtree of " << u << " fin" << endl;
#endif
      update(u, f, i);
#ifdef DEBUG
      cout << "update " << u << " fin" << endl;
#endif
    };
    init(init, u, u, 0);
#ifdef DEBUG
    cout << "init finish" << endl;
#endif
    auto dfs = [&](auto &&self, int u, int f, int i) -> bool {
      if (go[u] != nil) {
        x[u] = y[u] = 0;
        makego(u, u, 0);
        return true;
      }
      for (auto [v, w] : e[u])
        if (v != f) {
          update(u, v, w);
          update(v, v, 0);
          if (self(self, v, u, w)) return true;
          update(v, u, w);
          update(u, u, 0);
        }
      return false;
    };
    return dfs(dfs, u, u, 0);
  }
};
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n, m;
    cin >> n >> m;
    vector<vector<int>> e(n + 1);
    while (m--) {
      int u, v;
      cin >> u >> v;
      e[u].push_back(v), e[v].push_back(u);
    }
    bool flg = false;  // degree at most 3
    for (auto &ed : e) flg = flg || ed.size() > 3;
    if (flg) {
      cout << "No" << el;
      continue;
    }
#ifdef DEBUG
    cout << "Graph:" << endl;
    for (int i = 1; i <= n; i++) {
      cout << i << ": ";
      for (auto v : e[i]) cout << ' ' << v;
      cout << endl;
    }
#endif
    e = block_cut(e);
    m = e.size() - 1;
#ifdef DEBUG
    cout << "block cut fin" << endl;
#endif
    tree tr;
    if (!tr.init(e, n)) {
      cout << "No" << el;
      continue;
    }
#ifdef DEBUG
    cout << "init fin" << endl;
#endif
    if (!tr.reroot(m)) {
      cout << "No" << el;
      continue;
    }
    cout << "Yes" << el;
    for (int i = 1; i <= n; i++) cout << tr.x[i] << ' ' << tr.y[i] << el;
  }
  return 0;
}
```