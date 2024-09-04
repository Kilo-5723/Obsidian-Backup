---
dg-publish: false
source: 
id: 
contest: 
problem: 
tags: 
lead: A very very very very very very very very very very very very very very  long sentence
---
```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
ll lowbit(ll n) { return n > 0 ? n & -n : 0; }
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int tt;
  cin >> tt;
  while (tt--) {
    int n, k;
    cin >> n >> k;
    vector<ll> a(n);
    for (auto &v : a) cin >> v;
    ll sum = 0;
    map<ll, int> cnt;
    for (auto v : a) sum ^= v, cnt[lowbit(v)]++;
    ll mxm = 0;
    for (auto v : a) {
      auto cur = sum ^ v ^ v + 1;
      ll mnm = 1e18;
      cnt[lowbit(v)]--, cnt[lowbit(v + 1)]++;
      for (auto [k, v] : cnt)
        if (v > 0 && k > 0) mnm = min(mnm, cur ^ k ^ k - 1);
      mxm = max(mxm, mnm);
      cnt[lowbit(v)]++, cnt[lowbit(v + 1)]--;
    }
    cout << mxm << el;
  }
  return 0;
}
```