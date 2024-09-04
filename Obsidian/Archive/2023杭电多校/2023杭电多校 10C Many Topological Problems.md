---
link: "[23HDU10C](http://acm.hdu.edu.cn/showproblem.php?pid=7385)"
done: true
tags:
  - Combinatorics
---

我们用 ${\rm good}(p,T)$ 表示排列 $p$ 是一棵给定的树 $T$ 的好排列，则题目要求为求出 $\sum\limits_T\sum\limits_p [{\rm good}(p,T)]$。

我们可以把求和顺序改为 $\sum\limits_p\sum\limits_T [{\rm good}(p,T)]$，即对每一个排列 $p$ 求出满足 ${\rm good}(p,T)$ 的 $T$ 的数量。我们可以发现 $\sum\limits_T[{\rm good}(p,T)]$ 对每个排列 $p$ 都相等。对任意两个排列 $p_1$, $p_2$，我们分别用  $\cal{T_1}$, $\cal{T_2}$ 表示满足 ${\rm good}(p_1,T)$, ${\rm good}(p_2,T)$ 的两个集合，则 $\cal{T_1}$ 和 $\cal{T_2}$ 里的元素是一一对应的，因为我们可以把任何 $T\in\cal{T_1}$ 根据 $p_1$ 和 $p_2$ 重编号为一棵唯一的 $T'\in\cal{T_2}$，反之亦然。因此，答案即为 $n!\cdot \sum\limits_T[{\rm good}([1,\dots,n],T)]$。

为了找到满足 ${\rm good}([1,\dots,n],T)$ 的 $T$ 的数量，我们只需要对每一个 $i$ 找到其树上的父亲，即 $par_i$。我们有：
- $1$ 是树的根结点。
- 对于 $2\le i\le k$，$par_i$ 可能在 $[1,\dots,i-1]$ 之间。
- 对于 $i>k$，$par_i$ 可能在 $[i-k,\dots,i-1]$ 之间。

根据乘法原理，满足 ${\rm good}([1,\dots,n],T)$ 的 $T$ 的数量即为 $(k-1)!\cdot k^{n-k}$，则原问题的答案为 $n!\cdot (k-1)!\cdot k^{n-k}$。