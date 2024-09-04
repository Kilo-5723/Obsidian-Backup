---
dg-publish: true
source: Codeforces
contest: Codeforces Round 804 (Div. 2)
cid: "1699"
problem: E
name: Three Days Grace
tags:
  - todo
  - mathematics
  - dynamic-programming
  - harmonic-series
lead:
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

$
\newcommand{\mfac}{\mathop{\rm minfact}}
$给出一个大小为 $n$ 的集合 $S=\{a_1,\dots,a_n\}$，其中 $1\le a_i\le m$。每一次操作可以删除 $S$ 中的一个元素 $a_i$，将 $a_i$ 分解成 $a_i=p\cdot q$ 并将 $p$ 和 $q$ 加入 $S$。操作可以执行任意多次，求最小的 $\max S-\min S$。

数据范围 $1\le n\le 10^6$，$1\le m\le 5\cdot 10^6$。
### 题解

|           记号 | 含义                                                                                                                  |
| -----------: | :------------------------------------------------------------------------------------------------------------------ |
| $\mfac(n,l)$ | $\min\{r \mid n=\prod_{i=1}^k b_i, l\le b_i\le r\}$，即 $n$ 分解为不小于 $l$ 的因子时最大因子的最小值。特殊地， $l>n$ 时 $\mfac(n,l)=+\infty$ |
我们定义 $\mfac(n,l)=\min\{r \mid n=\prod_{i=1}^k b_i, l\le b_i\le r\}$，即 $n$ 分解为不小于 $l$ 的因子时最大因子的最小值。特殊地，当 $l>n$ 时 $\mfac(n,l)=+\infty$.我们有：
$$\mfac(n,l)=\begin{cases}
\min\{\mfac(n/l,l),\mfac(n,l+1)\} & l\mid n \\
\mfac(n,l+1) & l\nmid n
\end{cases}$$
注意到从 $\mfac(i,l+1)$ 更新到 $\mfac(i,l)$ 时只需要更新 $\lfloor n/l \rfloor$ 个位置，因此如果从大到小枚举 $\min S$，则可以在 $O(m\log m)$ 的时间里