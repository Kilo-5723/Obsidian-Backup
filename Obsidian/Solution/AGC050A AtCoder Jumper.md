---
dg-publish: true
source: AtCoder
contest: AtCoder Grand Contest 050
cid: agc050
problem: A
name: AtCoder Jumper
tags:
  - draft
  - construction
lead: 别想着树形结构，想想位运算就赢了
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

构造一个 $n$ 个点的有向图，使得每个点出度为 $2$，同时任意两点的单向距离都 $\le 10$。

数据范围 $n\le 1000$。

### 题解

很容易陷入先构造一棵树然后考虑叶子节点如何连边的陷阱里。

考虑位运算，发现只需要 $u\ll 1$ 和 $u\ll 1\mid1$ 两种操作就能在 $10$ 步以内构造出任何 $10$ 位二进制数，这样连边即可。注意需要分类讨论 $n$ 不为 $2$ 的幂次时最高位的情况。

```cpp
#include <bits/stdc++.h>
using namespace std;
const char el = '\n';
typedef long long ll;
int main() {
  ios::sync_with_stdio(false);
  cin.tie(0);
  cout << setprecision(15);
  int n;
  cin >> n;
  int high = 1 << __lg(n);
  int mask = high - 1;
  auto f = [&](int x) {
    if (x < n) return x;
    if ((x & high) && (x & mask) + high < n) return (x & mask) + high;
    return x & mask;
  };
  for (int i = 0; i < n; i++)
    cout << f(i << 1) + 1 << ' ' << f(i << 1 | 1) + 1 << el;
  return 0;
}
```