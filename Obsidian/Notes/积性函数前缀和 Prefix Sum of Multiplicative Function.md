---
dg-publish: true
done: false
tags:
  - number-theory
---
# 积性函数前缀和
$
\newcommand{\idiv}[2]{{\lfloor{#1}/{#2}\rfloor}}
\newcommand{\lpf}{\mathop{\rm lpf}}
\newcommand{\pow}{\mathop{\rm pow}}
$给出一个[积性函数](https://en.wikipedia.org/wiki/Multiplicative_function) $f(x)$ 和上界 $n$。

求 $\sum_{i=1}^nf(i)$。时间复杂度 $o(n)$。

|          记号 | 含义                                                                          |
| ----------: | :-------------------------------------------------------------------------- |
|         $P$ | 全体素数集合（不包括 $1$）                                                             |
|       $p_k$ | 第 $k$ 小的素数（特殊地，$p_0=1$）                                                     |
|   $\lpf(n)$ | $\min\{p : p\mid n\}$， $n$ 的最小素因子（特殊地，${\rm lpf}(1)=1$）                     |
| $\pow(p,n)$ | $\max\{c : p^c\mid n\}$，$n$ 中 $p$ 的次数                                       |
|       $[P]$ | [艾佛森括号](https://en.wikipedia.org/wiki/Iverson_bracket)，$P$ 为真时为 $1$，否则为 $0$ |
|      $f(x)$ | 问题中待求和的函数                                                                   |
|    $F_k(n)$ | $\sum_{i=2}^n[{\rm lpf}(i)\ge p_k] f(i)$，最小素因子大于 $p_k$ 的 $f(x)$ 前缀和         |
|    $F_P(n)$ | $\sum_{p\le n, p\in P} f(p)$，素数的 $f(x)$ 前缀和                                 |

# Min25 筛

Min25 筛要求 $f(n)$ 满足下列条件：
1. $f(p)$ 是一个项数较少的多项式
2. $f(p^k)$ 可以快速求值


Min25 筛的时间复杂度为 $O(n^{3/4}/\log n)$，空间复杂度为 $O(\sqrt{n})$。其解决这个问题的思路基于这样一个事实：$1$ 到 $n$ 中的所有合数的最小素因子 $\le\sqrt{p}$。因此，将 $1$ 到 $n$ 的所有整数按最小素因子 $p$ 及其次数 $c$ 分类。这样会出现两种情况：
1. $p\le \sqrt{n}$，这样的 $(p,c)$ 有 $O(\sqrt{n})$ 组，因为每个小于 $\sqrt{n}$ 的数只对应一组 $(p,c)$，而将 $c$ 翻倍就能覆盖到所有 $n$ 以内的数
2. $p>\sqrt{n}$，这样的 $c$ 只能为 $1$。更进一步，被归为此类的整数都是大素数

根据积性函数的定义，对于情况 1，我们可以将每组 $(p,c)$ 中 $f(p^c)$ 的贡献提出来，剩余部分则对应了一个稍有不同的计数问题：我们要对 $2$ 到 $\idiv{n}{p^c}$ 的所有最小素因子大于 $p$ 的数，求出 $f(x)$ 的和。因此，我们定义：
$$F_k(n)=\sum_{i=2}^n[\lpf(i)\ge p_k] f(i)$$

而对于情况 2，不妨将情况 1 中的素数也提取出来，则我们需要对 $2$ 到 $n$ 的所有素数求出 $f(x)$ 的和。考虑到情况 1 将问题递归到了 $\lpf(i)\ge k$ 的形式，对于素数相当于对 $p_{k+1}$ 到 $n$ 的素数求 $f(x)$ 的和，这相当于两个前缀和相减。因此，我们定义：
$$F_P(n)=\sum_{\substack{p\le n\\ p\in P}} f(p)$$

显然，在这样的定义下 $\sum_{i=1}^n f(i)=F_1(n)+f(1)=F_1(n)+1$。这样，我们把原问题转化为了 $O(\sqrt{n})$ 个情况 1 的子问题和两个情况 2 的子问题。接下来，我们将上面的分析转化为对 $F_k(n)$ 求值的递推式。

首先，按最小素因子及其次数分类：
$$\begin{align*}
F_k(n) & = \sum_{i=2}^n[p_k\le\lpf(i)]f(i) \\

& = \sum_{i=2}^n[p_k\le \lpf(i)]f(i/\lpf(i)^{\pow(\lpf(i),i)})\cdot f(\lpf(i)^{\pow(\lpf(i),i)}) \\

& = \sum_{j\ge k}\sum_{c\ge 1} f(p_j^c) \cdot \sum_{i=2}^n [\lpf(i)=p_j\wedge\pow(\lpf(i),i)=c] f(i/p_j^c) \\

& = \sum_{j\ge k}\sum_{c\ge 1} f(p_j^c) \cdot \sum_{i=1}^\idiv{n}{p^c} [i=1\vee \lpf(i)>p_j] f(i) \\

& = \sum_{j\ge k}\sum_{c\ge 1} f(p_j^c) \cdot \left(1+F_{j+1}\left(\idiv{n}{p_j^c}\right)\right) \\
\end{align*}$$
然后，将所有对素数的求和提取出来：
$$\begin{align*}
F_k(n) & = \sum_{\substack{j\ge k\\p_j^2\le n}}\sum_{c\ge 1} f(p_j^c) \cdot \left([c>1]+F_{j+1}(\idiv{n}{p_j^c})\right) + F_P(n)-F_P(p_{k-1}) \\

& = \sum_{\substack{j\ge k\\p_j^2\le n}}\sum_{c\ge 1} f(p_j^c) \cdot F_{j+1}(\idiv{n}{p_j^c})+\sum_{\substack{j\ge k\\p_j^2\le n}}\sum_{c\ge 2}f(p_j^c)+F_P(n)-F_P(p_{k-1}) \\

& = \sum_{\substack{j\ge k\\p_j^2\le n}}\sum_{\substack{c\ge 1\\p_j^{c+1}\le n}} \left(f(p_j^c) \cdot F_{j+1}(\idiv{n}{p_j^c})+f(p_j^{c+1})\right)+F_P(n)-F_P(p_{k-1}) \\
\end{align*}$$

最终，我们将 $F_k(n)$ 的值转化为对：
1. $O(\sqrt{n})$ 个 $F_{j+1}(\idiv{n}{x})$
2. $O(\sqrt{n})$ 个 $f(p^c)$
3. $F_P(n)-F_P(p_{k-1})$

的求和，其中 1. 可以递归求值，2. 在 Min25 筛的条件中保证可以快速求值，而接下来我们需要对 3. 中的 $F_P(n)$ 进行求值。

观察 $F_k(n)$ 的递归式，我们发现 $F_P(n)$ 的求值只在 $\idiv{n}{i}$ 处和 $p_k$ 处进行，其中 $p_k$ 在递归时不会超过 $\sqrt{n}$。因此，我们实际上只需要 $F_P(n)$ 在 $1,2,\dots,\lfloor\sqrt{n}\rfloor$ 和 $\idiv{n}{\lfloor\sqrt{n}\rfloor},\cdots,\idiv{n}{2},n$ 这 $O(\sqrt{n})$ 处的点值。

## 施工中

# References

1. 朱震霆，一些特殊的数论函数求和问题，IOI2018 中国国家候选队论文集