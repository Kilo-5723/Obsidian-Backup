---
dg-publish: true
source: QOJ
contest: The 2nd Universal Cup Semifinals
cid: "1708"
problem: B
pid: "8814"
name: Almost Convex 2
tags:
  - geometry
  - draft
lead: 计算几何，对凸包内一点极角排序预处理点边构成三角形内部点数，然后分类讨论
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header2(dv);
```

### 题解

显然答案为凸包 $P$ 加上至多两个内点。注意到保证三点不共线，对每个内点进行极角排序，预处理每个内点和凸包每条边 $p_i$ 构造的三角形里有几个其他点，这样凸包加一个点就一定是对一个内点 $u$ 选择条凸包的边 $p_i$ 使得 $u,p_i$ 形成的三角形内不存在其他点。对于加两个点 $u,v$ 的情况，分类讨论：

1. $u,v$ 选择不同的边 $p_i,p_j$，只需要这两个三角形都不存在其他点且三角形不相交，用前缀和求和
2. $u,v$ 选择同一条边 $p_i$ 且 $p_i$ 和直线 $uv$ 没有交点，要求 $u,v,p_i$ 的四边形区域内没有其他点，点数的计算可以先暴力算出第一个四边形 $u,v,p_0$ 内的内点数，然后加减预处理过的三角形来 $O(1)$ 算出下一个四边形
3. $u,v$ 选择同一条边 $p_i$ 且 $p_i$ 和直线 $uv$ 有交点，这样的 $p_i$ 一共有两个，各自枚举 $u,v$ 和 $p_i$ 的哪个端点相连一共出现四种情况，分别暴力即可

总时间复杂度为 $O(n^3)$。

### 注意事项

对于第三种情况，记得把 $u,v$ 从另一个点要求的三角形区域内删掉

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
struct cplx {
  int x, y;
  ll abs2() { return x * x + y * y; }
};
cplx operator+(const cplx &a, const cplx &b) { return {a.x + b.x, a.y + b.y}; }
cplx operator-(const cplx &a, const cplx &b) { return {a.x - b.x, a.y - b.y}; }
bool operator<(const cplx &a, const cplx &b) {
  return a.x != b.x ? a.x < b.x : a.y < b.y;
}
bool operator!=(const cplx &a, const cplx &b) { return a < b || b < a; }
bool operator==(const cplx &a, const cplx &b) { return !(a != b); }
int dot(const cplx &a, const cplx &b) { return a.x * b.x + a.y * b.y; }
int det(const cplx &a, const cplx &b) { return a.x * b.y - a.y * b.x; }
vector<cplx> diagsort(vector<cplx> &a, cplx o) {
  sort(a.begin(), a.end(), [&](const cplx &p, const cplx &q) {
    if ((o < p) != (o < q)) return o < p;
    auto d = det(p - o, q - o);
    if (d) return d > 0;
    return (p - o).abs2() > (q - o).abs2();
  });
  auto k = unique(a.begin(), a.end(),
                  [&](const cplx &p, const cplx &q) {
                    return !det(p - o, q - o) && dot(p - o, q - o) > 0;
                  }) -
           a.begin();
  vector<cplx> res = {a.begin() + k, a.end()};
  a.resize(k);
  return res;
}
void diagsort(vector<pair<cplx, int>> &a, cplx o) {
  sort(a.begin(), a.end(),
       [&](const pair<cplx, int> &p, const pair<cplx, int> &q) {
         if ((o < p.first) != (o < q.first)) return o < p.first;
         auto d = det(p.first - o, q.first - o);
         if (d) return d > 0;
         return (p.first - o).abs2() > (q.first - o).abs2();
       });
}
pair<vector<cplx>, vector<cplx>> convex(vector<cplx> a) {
  sort(a.begin(), a.end());
  auto o = a[0];
  a.erase(a.begin());
  auto erase = diagsort(a, o);
  vector<cplx> res = {o};
  for (auto v : a) {
    while (res.size() >= 2) {
      auto p = res[res.size() - 2], q = res[res.size() - 1];
      if (det(q - p, v - q) > 0) break;
      erase.push_back(res.back()), res.pop_back();
    }
    res.push_back(v);
  }
  return {res, erase};
}

cplx find(vector<cplx> &conv, cplx a) {
  int l = 0, r = conv.size() - 1;
  while (r - l) {
    int m = (l + r) / 2;
    if (det(conv[m + 1] - conv[m], a) < 0)
      r = m;
    else
      l = m + 1;
  }
  return conv[l];
}
ll solve(vector<ll> &a) {
  vector<ll> s = {0};
  for (auto v : a) s.push_back(s.back() + v);
  vector<cplx> conv;
  for (int i = 0; i < s.size(); i++) {
    cplx r = {i, s[i]};
    while (conv.size() >= 2) {
      cplx p = conv[conv.size() - 2], q = conv.back();
      if (det(q - p, r - q) > 0) break;
      conv.pop_back();
    }
    conv.push_back(r);
  }
  ll res = 0;
  for (int i = 0; i < a.size(); i++) {
    auto p = find(conv, {1, a[i]});
    res = max(res, s[i] - p.y + (p.x - i) * a[i]);
  }
  return res;
}
bool upside(cplx p, cplx q, cplx a) { return det(q - p, a - p) > 0; }
ll solve(vector<cplx> &a, vector<cplx> &b, int p, int q, vector<int> &belp,
         vector<int> &belq, vector<int> &cntp, vector<int> &cntq) {
  int belpq = belp[q], belqp = belq[p];
  belp[q] = -1, belq[p] = -1;
  cntp[belpq]--, cntq[belqp]--;
  int n = a.size(), m = b.size();
  int x, y;
  auto next = [&](int i) { return (i + 1) % n; };
  auto prev = [&](int i) { return (i + n - 1) % n; };
  for (int i = 0; i < n; i++)
    if (upside(b[p], b[q], a[i])) {
      if (!upside(b[p], b[q], a[next(i)])) x = next(i);
    } else {
      if (upside(b[p], b[q], a[next(i)])) y = next(i);
    }
  ll res = 0;
  int cnt = 0;
  int px = prev(x), py = prev(y), p2x = prev(px), p2y = prev(py);
  for (int i = 0; i < n; i++) cnt += cntq[i] == 0;
  cnt -= cntq[px] == 0;
  int cur = cnt;
  for (int i = x; i != py; i = next(i)) {
    cur -= cntq[i] == 0;
    if (cntp[i] == 0) res += cur;
  }
  cur = cnt;
  for (int i = p2x; i != py; i = prev(i)) {
    cur -= cntq[i] == 0;
    if (cntp[i] == 0) res += cur;
  }
  if (cntp[px] == 0) res += cnt;
  // cout << "part 1 = " << res << el;
  int qpx = 0, qxx = 0, ppy = 0, pyy = 0;
  int ax = 0, ay = 0;
  for (int i = 0; i < m; i++) {
    if (belq[i] == px) upside(b[p], b[q], b[i]) ? qpx++ : qxx++;
    if (belp[i] == py) upside(b[p], b[q], b[i]) ? pyy++ : ppy++;
    if (belp[i] == px && !upside(b[p], b[q], b[i])) ax++;
    if (belq[i] == py && upside(b[p], b[q], b[i])) ay++;
  }
  if (cntp[px] == 0) res += (qpx == 0) + (qxx == 0);
  if (cntq[py] == 0) res += (ppy == 0) + (pyy == 0);
  cnt = qxx - ax;
  for (int i = x; i != py; i = next(i)) {
    cnt += cntq[i];
    if (cnt == 0) res++;
    cnt -= cntp[i];
  }
  cnt = pyy - ay;
  for (int i = y; i != px; i = next(i)) {
    cnt += cntp[i];
    if (cnt == 0) res++;
    cnt -= cntq[i];
  }
  // cout << res << el;
  belp[q] = belpq, belq[p] = belqp;
  cntp[belpq]++, cntq[belqp]++;
  return res;
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n;
  while (cin >> n) {
    vector<cplx> a(n);
    for (auto &[x, y] : a) cin >> x >> y;
    auto [aa, b] = convex(a);
    a = aa;
    int m = b.size();
    // cout << "convex: ";
    // for (auto v : a) cout << v.x << ' ' << v.y << " ; ";
    // cout << el;
    // cout << "others: ";
    // for (auto v : b) cout << v.x << ' ' << v.y << " ; ";
    // cout << el;
    n = a.size();
    vector bel(m, vector<int>(m, -1));
    vector cnt(m, vector<int>(n, 0));
    for (int i = 0; i < b.size(); i++) {
      vector<pair<cplx, int>> arr;
      for (auto v : a) arr.push_back({v, -1});
      for (int j = 0; j < b.size(); j++)
        if (i != j) arr.push_back({b[j], j});
      diagsort(arr, b[i]);
      for (int j = 0; j < arr.size(); j++)
        if (arr[j] == make_pair(a[0], -1)) {
          arr.insert(arr.end(), arr.begin(), arr.begin() + j);
          arr.erase(arr.begin(), arr.begin() + j);
          break;
        }
      // for (auto [c, id] : arr) cout << c.x << ' ' << c.y << ' ' << id << " ;
      // "; cout << el;
      int cur = -1;
      for (auto [c, id] : arr)
        if (~id) {
          bel[i][id] = cur, cnt[i][cur]++;
        } else
          cur++;
    }
    // for (auto arr : bel) {
    //   for (auto v : arr) cout << v << ' ';
    //   cout << endl;
    // }
    ll ans = 1;
    for (auto arr : cnt)
      for (auto v : arr) ans += v == 0;
    // cout << a.size() << ' ' << b.size() << el;
    for (int i = 0; i < m; i++)
      for (int j = i + 1; j < m; j++) {
        ans += solve(a, b, i, j, bel[i], bel[j], cnt[i], cnt[j]);
        // cout << i << ' ' << j << ' ' << ans << el;
      }
    cout << ans << el;
  }
  return 0;
}
```