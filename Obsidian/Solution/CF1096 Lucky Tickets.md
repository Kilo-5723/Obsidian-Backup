---
dg-publish: true
source: Codeforces
contest: Educational Codeforces Round 57
cid: "1096"
problem: G
name: Lucky Tickets
tags:
  - draft
  - convolution
  - inclusion-exclusion
lead: NTT 板子或者容斥，也许可以生成函数？
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

#### NTT

NTT 快速幂，做一次 $(nk/2)$ 长的 NTT 变换，对每一项做 $n/2$ 次快速幂，再变换回去，复杂度 $O(nk\log nk)$。

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
const ll mod = 998244353;
typedef vector<ll> poly;
ll qpow(ll a, ll b) {
  ll res = 1;
  while (b) {
    if (b & 1) res = res * a % mod;
    a = a * a % mod;
    b >>= 1;
  }
  return res;
}
ll inv(ll n) { return qpow(n, mod - 2); }
const auto pi = acosl(-1);
const int len = 15, mask = (1 << len) - 1;
vector<ll> get_root(int n) {
  n = 1 << 32 - __builtin_clz(n);
  int mul = qpow(3, (mod - 1) / n);
  vector<ll> res{1};
  res[0] = 1;
  for (int i = 1; i < n; i++) res.push_back(res.back() * mul % mod);
  return res;
}
void fft(vector<ll> &p, const vector<ll> &w) {
  int n = w.size();
  for (int i = 1, j = 0; i < n - 1; ++i) {
    int s = n;
    do s >>= 1, j ^= s;
    while (~j & s);
    if (i < j) swap(p[i], p[j]);
  }
  for (int d = 0; (1 << d) < n; ++d) {
    int m = 1 << d, m2 = m * 2, rm = n >> (d + 1);
    for (int i = 0; i < n; i += m2)
      for (int j = 0; j < m; ++j) {
        auto &p1 = p[i + j + m], &p2 = p[i + j];
        p1 %= mod, p2 %= mod;
        auto t = w[rm * j] * p1 % mod;
        p1 = (p2 - t) % mod;
        p2 = (p2 + t) % mod;
      }
  }
}
poly qpow(poly a, int b) {
  int m = a.size();
  vector<ll> w = get_root(m);
  int n = w.size();
  a.resize(n), fft(a, w);
  for (int i = 0; i < n; i++) a[i] = qpow(a[i], b);
  fft(a, w), reverse(a.begin() + 1, a.end());
  a.resize(m);
  ll invn = qpow(inv(n), 1);
  for (auto &v : a) v = (v * invn % mod + mod) % mod;
  return a;
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n, k;
  cin >> n >> k;
  poly p(n * 5 + 1);
  while (k--) {
    int v;
    cin >> v;
    p[v] = 1;
  }
  auto a = qpow(p, n / 2);
  ll ans = 0;
  for (auto v : a) ans += v * v % mod;
  cout << ans % mod << el;
  return 0;
}
```