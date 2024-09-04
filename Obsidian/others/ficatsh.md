# Single Round Match 720 Hard ExtremeSpanningTrees 解法研究

题意：给出 $50$ 个点，$1000$ 条边的带权无向图和它的两棵生成树 $T_1$ 和 $T_2$.
你可以花 $1$ 的代价使某条边的 $\pm 1$，求使得 $T_1$ 变成最小生成树，$T_2$ 变成最大生成树的最小代价。

如果只有 $T_1$，那么就是 KM 经典题 [Roads](http://acm.zju.edu.cn/onlinejudge/showProblem.do?problemCode=2342). 同时有 $T_1$ 和 $T_2$，情况有些不同。

Petr 在[这个评论](http://codeforces.com/blog/entry/54020?#comment-380695) 中提到了两篇 paper，本文的余下的内容就是这两篇论文中的算法。

这两篇论文解决的都是更加一般的，称为 Isotonic Regression 的问题。
即：给出 DAG $G$ 和 $n$ 个数 $a_1, a_2, \ldots, a_n$，要求 $x_1, x_2, \ldots, x_n$ 满足如果 $\langle i, j \rangle \in G$，那么 $x_i \leq x_j$，同时最小化 $\sum_{i = 1}^n |a_i - x_i|$.

## Isotonic Regression via Partitioning

Stout, Quentin F.. “Isotonic Regression via Partitioning.” Algorithmica 66 (2012): 93-112.

这篇论文基于这样一个性质，假设问题的最优解是 $x^*_i$。
如果我们限制 $x_i \in \{m, m + 1\}$，求得的最优解是 $x^+_i$。
那么 $x^*_i \leq m$ 等价于 $x^+_i = m$.

增加了新限制的问题就是常见的最大权闭合子图，可以用网络流解决。之后就可以递归处理两边的子问题，确定每个 $x$ 的值。

下面解释这个性质。如果这个性质不对，考虑所有 $x^+_i = m + 1$ 的点，假设是 $C$.
这个 $C$ 显然是一个闭合子图。
其中有一些点 $x^*_i \leq m$，假设是 $D$. 那么 $C \setminus D$ 到 $D$ 是没有边的，也就是说 $C \setminus D$ 也是一个闭合子图。
因为 $C$ 是最小权的闭合子图，所以 $D$ 的权值之和肯定是 $\leq 0$ 的。而绝对值函数是非负的，也就是说，所有的 $i \in D$ 都有 $a_i = (m + 1)$.
那么把这些 $i$ 的 $x^*_i$ 增加到 $(m + 1)$，肯定能让目标函数变小。同时这么做也是合法的，因为 $D$ 只有指向 $C \setminus D$ 的边，没有指向 $C$ 外部的边（因为 $C$ 是闭合子图），而 $C \setminus D$ 里面的点都是 $\geq m + 1$ 的，所以这样不会违反限制。

## Weighted isotonic regression under the $L_1$ norm

Angelov, Stanislav et al. “Weighted isotonic regression under the L1 norm.” SODA '06 (2006).

这篇论文就套路多了。。。考虑线性规划，假设元素 $i$ 的终值是 $x_i$，变化量是 $e_i$.
那么有 $e_i + x_i \geq a_i$ ($s_i$), 同时 $e_i - x_i \geq -a_i$ ($t_i$).
DAG 的限制是 $x_i \geq x_j$ ($r_{ij}$).
最后要最小化 $\sum_i e_i$.

考虑对偶，对 $e_i$ 可以列出方程 $s_i + t_i \leq 1$.
对 $x_i$ 可以列出方程
$$ s_i - t_i + \sum_{j} r_{ij} - \sum_{j} r_{ji} \geq 0.$$
最后要最小化 $\sum_i a_i (s_i - t_i)$.

这里有个小技巧，就是考虑 complementary slackness.
对于最优解我们有
$$ x_i (s_i - t_i + \sum_{j} r_{ij} - \sum_{j} r_{ji}) = 0.$$
我们可以认为 $x_i > 0$，所以实际上会取到等号。

剩下的事情就简单了，可以看出这个取等的就是流量平衡条件，建个费用流模型就好。
可能有个 trick 是 $s_i, t_i$ 总有一个是 $0$，所以只要保证 $s_i, t_i \leq 1$ 就行。


[ftiasch wrote on 2017-08-26T10:52:54]

---