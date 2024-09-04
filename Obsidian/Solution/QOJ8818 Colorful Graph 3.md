---
dg-publish: true
source: QOJ
contest: The 2nd Universal Cup Semifinals
cid: "1708"
problem: F
pid: "8818"
name: Colorful Graph 3
tags:
  - draft
  - graph
  - construction
lead: 构造，关键性质是 ci=1 的边在其他边缩点后构成完全图
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header2(dv);
```

### 题解

显然边数至少为 $n-1$。

1. 如果存在 $c_k\ge 2$，只要用这种颜色染一个菊花图
2. 如果 $c_k=1$ 的数量 $\ge n-1$，可以各出一条边构造一棵树
3. 否则，对任意一个 $c_k=1$，将其他所有边缩点后剩下的一定是一个完全图

展开讨论第三种情况，考虑枚举这个完全图的大小，显然对不同的 $c_k$ 差距过大没有意义

如果还有点剩下可以用所有颜色构造一个环接在任意一点上。

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const int inf = 1e9;
struct edge {
  int u, v, w;
};
bool addcycle(vector<edge> &res, int &m, int n, vector<int> &a) {
  if (m == n) return false;
  int k = res.size();
  for (int i = 0; i < a.size() && m <= n; i++)
    res.push_back({m, m + 1, a[i]}), m++;
  res[k].u = res.back().v = 1, m--;
  return true;
}
bool calccycle(int &res, int &m, int n, vector<int> &a) {
  if (m == n) return false;
  for (int i = 0; i < a.size() && m <= n; i++) res++, m++;
  m--;
  return true;
}
int calc(int n, int k, vector<int> &d0, vector<int> &d1, vector<int> &d) {
  int res = 0;
  int cur = k - k * (k - 1) / 2;
  for (auto v : d1)
    for (int i = 0; i < k && cur < n; i++)
      for (int j = i + 1; j < k && cur < n; j++) res++, cur++;
  for (auto v : d0)
    for (int i = 1; i < k && cur < n; i++)
      for (int j = i + 1; j < k && cur < n; j++) res++, cur++;
  while (cur < n) calccycle(res, cur, n, d);
  return res;
}
vector<edge> solve(int n, int k, vector<int> &d0, vector<int> &d1,
                   vector<int> &d) {
  vector e(k, vector(k, vector<int>()));
  int cur = k - k * (k - 1) / 2;
  for (auto v : d1)
    for (int i = 0; i < k && cur < n; i++)
      for (int j = i + 1; j < k && cur < n; j++) e[i][j].push_back(v), cur++;
  for (auto v : d0)
    for (int i = 1; i < k && cur < n; i++)
      for (int j = i + 1; j < k && cur < n; j++) e[i][j].push_back(v), cur++;
  vector<edge> res;
  while (cur < n) addcycle(res, cur, n, d);
  cur = k;
  for (int i = 0; i < k; i++)
    for (int j = i + 1; j < k; j++) {
      int t = res.size();
      for (auto v : e[i][j]) res.push_back({cur, cur + 1, v}), cur++;
      res[t].u = i + 1, res.back().v = j + 1, cur--;
    }
  return res;
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
    vector<int> a(k);
    for (auto &v : a) cin >> v;
    // exist ck >= 2
    bool flg = false;
    for (int i = 0; i < k; i++)
      if (a[i] > 1) {
        flg = true;
        cout << n - 1 << el;
        for (int j = 2; j <= n; j++) cout << 1 << ' ' << j << ' ' << i + 1 << el;
        break;
      }
    if (flg) continue;
    vector<int> d0, d1;
    for (int i = 0; i < k; i++) (a[i] ? d1 : d0).push_back(i);
    // |ck = 1| >= n-1
    if (d1.size() >= n - 1) {
      cout << n - 1 << el;
      for (int i = 1; i < n; i++)
        cout << i << ' ' << n << ' ' << d1[i - 1] + 1 << el;
      continue;
    }
    vector<edge> ans;
    vector<int> d = d0;
    for (int i = 0; i < 3; i++) d.insert(d.end(), d1.begin(), d1.end());
    int cur = 1;
    addcycle(ans, cur, n, d);
    d.resize(d0.size() + d1.size());
    while (cur < n) addcycle(ans, cur, n, d);
    // enumerate size of complete graph
    vector<int> tmp;
    for (int i = 2; i * (i - 1) / 2 <= 2 * n; i++) tmp.push_back(i);
    reverse(tmp.begin(), tmp.end());
    if (d1.size())
      for (auto i : tmp) {
        if (calc(n, i, d0, d1, d) < ans.size()) ans = solve(n, i, d0, d1, d);
      }
    cout << ans.size() << el;
    for (auto [u, v, w] : ans) cout << u << ' ' << v << ' ' << w + 1 << el;
  }
  return 0;
}
```