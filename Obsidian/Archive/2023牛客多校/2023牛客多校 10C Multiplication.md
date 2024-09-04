---
link: "[23NC10C](https://ac.nowcoder.com/acm/contest/57364/C)"
done: true
tags:
  - big-integer
  - brute-force
---

首先，我们将循环数的条件形式化。我们用 $a$ 和 $b$ 表示循环数的左边和右边，用 $p$ 和 $q$ 分别表示 $a$ 和 $b$ 的长度，则题目描述中的条件可以被转化为以下四个条件：
1. $\left(a\cdot 10^q+b\right)\cdot k=b\cdot 10^p+a$，基于循环数的定义。
2. $a\cdot 10^q+b\le n$，基于题目的要求。
3. $a<10^p$ 且 $b<10^q$，因为 $a$ 和 $b$ 的长度不能超过 $p$ 和 $q$。 
4. $a\ge 10^{p-1}$ 且 $b\ge 10^{q-1}$，因为 $a$ 和 $b$ 不能包含前导零。 

基于条件 1，我们有
$$\begin{align}
\left(a\cdot 10^q+b\right)\cdot k&=b\cdot 10^p+a \\
\frac{a}{b}&=\frac{10^p-k}{k\cdot 10^q-1} \\
\end{align}$$
令 $\displaystyle \frac{x}{y}$ 为 $\displaystyle \frac{10^p-k}{k\cdot 10^q-1}$ 的最简分式，则如果 $a$, $b$ 存在，那么一定存在某个正整数 $c$ 使得 $a=cx$ 且 $b=cy$。

因为 $n\le 10^{100}$，我们可以直接枚举所有合法的 $p$ 和 $q$ 并找到对应的 $x$ 和 $y$。而对于条件 2, 3, 4，我们可以通过大整数四则运算找到满足条件 2, 3 的最大的 $c$ 并检查其是否满足条件 4。可以使用 Python 进行大整数运算。