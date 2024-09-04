---
dg-publish: true
source: 
id: 
contest: 
problem: 
tags:
---
# Problem

## [Contest Name](), [Problem X]()


```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
typedef double ld;
const ld eps = 1e-9, pi = acosl(-1);
struct cplx {
  ld x, y;
  ld abs2() const { return x * x + y * y; }
  ld abs() const { return sqrt(abs2()); }
  cplx r90() { return {-y, x}; }
};
cplx operator+(const cplx &a, const cplx &b) { return {a.x + b.x, a.y + b.y}; }
cplx operator-(const cplx &a, const cplx &b) { return {a.x - b.x, a.y - b.y}; }
cplx operator*(const cplx &a, ld b) { return {a.x * b, a.y * b}; }
cplx operator/(const cplx &a, ld b) { return {a.x / b, a.y / b}; }
cplx one(const cplx &a) { return a / a.abs(); }
bool operator<(const cplx &a, const cplx &b) {
  return a.x != b.x ? a.x < b.x : a.y < b.y;
}
bool operator!=(const cplx &a, const cplx &b) { return a < b || b < a; }
bool operator==(const cplx &a, const cplx &b) { return !(a == b); }
ld dot(const cplx &a, const cplx &b) { return a.x * b.x + a.y * b.y; }
ld det(const cplx &a, const cplx &b) { return a.x * b.y - a.y * b.x; }
random_device r;
default_random_engine e(r());
uniform_real_distribution<> g(0.0, 1.0);
double runtime() { return 1.0 * clock() / CLOCKS_PER_SEC; }
bool intime() { return runtime() < 0.9; }
struct pyramid {
  cplx o, a, b;
  ll h;
  pyramid(ld x1, ld y1, ld x2, ld y2, ld h) : a({x1, y1}), b({x2, y2}), h(h) {
    o = (a + b) / 2 + (b - a).r90() / 2;
  }
  pair<ld, cplx> foot(ld x) {
    cplx p{cos(x), sin(x)};
    int cnt = 4;
    while (det(a - o, p) < -eps || det(b - o, p) > eps) {
      cnt--, p = p.r90();
      // cout << cnt << endl;
    }
    auto d = (a + b) / 2 - o;
    p = p * d.abs2() / dot(d, p);
    // cout << (o + p).x << ' ' << (o + p).y << el;
    while (cnt--) p = p.r90();
    return {sqrt(p.abs2() + h * h), o + p};
  }
};
struct func {
  pyramid a, b;
  ld f(ld x1, ld x2) {
    auto [d1, p1] = a.foot(x1);
    auto [d2, p2] = b.foot(x2);
    return d1 + d2 + (p1 - p2).abs();
  }
};
ld simulate(func &f) {
  ld dx = 3;
  ld x1 = 0, x2 = 0, y = f.f(x1, x2), res = y;
  int cnt = 0;
  while (dx > 1e-10 && intime()) {
    cnt++;
    x1 -= (int)(x1 / 2 / pi) * 2 * pi, x2 -= (int)(x2 / 2 / pi) * 2 * pi;
    ld xx1 = x1 + dx * (g(e) * 2 - 1);
    ld yy = f.f(xx1, x2);
    double det = yy - y;
    res = min(res, yy);
    if (exp(-det / dx) > g(e)) x1 = xx1, y = yy;
    ld xx2 = x2 + dx * (g(e) * 2 - 1);
    yy = f.f(x1, xx2);
    det = yy - y;
    res = min(res, yy);
    if (exp(-det / dx) > g(e)) x2 = xx2, y = yy;
    dx *= 0.9999;
  }
  // cout << x1 << ' ' << x2 << endl;
  for (int i = 1; i <= 100 && intime(); i++) {
    ld xx1 = x1 + dx * (g(e) * 2 - 1), xx2 = x2 + dx * (g(e) * 2 - 1);
    res = min(res, f.f(xx1, xx2));
  }
  return res;
}
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int x1, y1, x2, y2, h;
  cin >> x1 >> y1 >> x2 >> y2 >> h;
  pyramid a(x1, y1, x2, y2, h);
  cin >> x1 >> y1 >> x2 >> y2 >> h;
  pyramid b(x1, y1, x2, y2, h);
  func f(a, b);
  cout << (double)simulate(f) << el;
  return 0;
}
```