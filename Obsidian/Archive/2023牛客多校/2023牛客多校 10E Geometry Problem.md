---
link: "[23NC10E](https://ac.nowcoder.com/acm/contest/57364/E)"
done: true
tags:
  - geometry
  - simulated-annealing
---

如果正方形一条边的角度 $\theta$ 已经给出，那么正方形的最小边长可以通过每个圆在 $(\cos\theta,\sin\theta)$ 和 $(\sin\theta,-\cos\theta)$ 两个方向上的投影直接算出。我们用 $f(\theta)$ 表示 $\theta$ 方向上的最小边长，则我们可以直接使用模拟退火找到 $\min f(\theta)$。

然而，我们无法证明模拟退火算法的正确性或正确概率，因此我们继续尝试寻找一个确定性算法。

$f(\theta)$ 和每个圆在 $(\cos\theta,\sin\theta)$ 和 $(\sin\theta,-\cos\theta)$ 两个方向上的投影相关。我们令圆 $c_i=(x_i,y_i,r_i)$ 在 $(\cos\theta,\sin\theta)$ 方向上的最大投影为 $g_i(\theta)$，令所有圆在 $(\cos\theta,\sin\theta)$ 方向上的最大投影为 $g(\theta)$，则有：
- $g_i(\theta)=x\cos\theta+y\sin\theta+r$。
- $g(\theta)=\max\limits_i g_i(\theta)$。
- $f(\theta)=\max\left(g(\theta)+g(\theta+\pi),g(\theta-\pi/2)+g(\theta+\pi/2)\right)$。

如果我们可以快速计算 $g(\theta)$ 并求出 $f(\theta)$ 的所有单峰区间，我们就可以在 $f(\theta)$ 的每个单峰区间内三分来找到 $\min f(\theta)$。

幸运的是，$g(\theta)$ 是一个至多有 $O(n)$ 段的分段函数，且每一段都是单峰函数，这是因为 $g(\theta)$ 和圆凸包等价。我们用 $H$ 代表所有圆形成的圆凸包，则所有圆在 $(\cos\theta,\sin\theta)$ 方向上的最大投影就是 $H$ 在 $(\cos\theta,\sin\theta)$ 方向上的最大投影。因此，对于 $H$ 上一段来自 $c_k=(x_k,y_k,r_k)$ 的弧 $a_i=(x_k+r_k\cos\theta,y+r_k\sin\theta)\; (\theta\in [\theta_l,\theta_r])$，我们有 $g(\theta)=g_k(\theta)$ $(\theta\in[\theta_l,\theta_r])$。

同时，$n$ 个圆形成的凸包上至多有 $2n-1$ 段弧，所以 $H$ 至多有 $2n-1$ 段弧，对应地 $g(\theta)$ 也至多有 $2n-1$ 段。另外，因为 $g_i(\theta)$ 只有两个峰值，所以任何一段 $g(\theta)$ 都可以被分为不超过两个单峰区间。因此，如果我们可以求出 $g(\theta)$，将 $g(\theta)$ 的每个单峰区间端点都映射到 $[0,\pi/2)$ 后，我们就可以找到 $f(\theta)$ 的全部 $O(n)$ 个单峰区间。

我们使用分治法求 $g(\theta)$。对于给定的圆集合 $C$，我们将其分为等大的两部分 $C_l$, $C_r$，递归求出 $g_l(\theta)=\max\limits_{i\in C_l}g_i(\theta)$ 和 $g_r(\theta)=\max\limits_{i\in C_r}g_i(\theta)$，再将 $g_l(\theta)$ 和 $g_r(\theta)$ 合并为 $g(\theta)=\max(g_l(\theta),g_r(\theta))$，合并步骤如下：
1. 对 $g_l(\theta)$ 和 $g_r(\theta)$ 的每一个分段点取并集，则对取并集后的每一段连续区间 $[\theta_i,\theta_{i+1}]$，$g_l(\theta)$ 和 $g_r(\theta)$ 都只有唯一的表达式，记为 $g_l(\theta)=x_l\cos\theta+y_l\sin\theta+r_l$ 和 $g_r(\theta)=x_r\cos\theta+y_r\sin\theta+r_r$。
2. $g_l(\theta)-g_r(\theta)$ 有至多两个零点，因此在区间 $[\theta_i,\theta_{i+1}]$ 上 $g(\theta)=\max(g_l(\theta),g_r(\theta))$ 至多分三段。对再次划分后的每一段区间 $[\theta'_i,\theta'_{i+1}]$，我们通过 $g_l(\theta)$ 和 $g_r(\theta)$ 在区间中点 $\theta'=(\theta'_i+\theta'_{i+1})/2$ 上的取值决定取哪一段作为 $g(\theta)$ 的表达式。若 $g_l(\theta')>g_r(\theta')$ 则令 $g(\theta)=g_l(\theta)$，否则令 $g(\theta)=g_r(\theta)$。
3. 为了保证 $g(\theta)$ 只有 $O(n)$ 段，我们需要合并相邻的同表达式区间。

我们可以在 $O(n\log n)$ 的时间内求出 $g(\theta)$，而基于不同的实现方式，对 $f(\theta)$ 每一段单峰区间的三分的时间复杂度是 $O(\log V)$ 或 $O(\log n\log V)$，其中 $V$ 是三分的值域大小。因此，整体的时间复杂度是 $O\left(n(\log n+\log V)\right)$ 或 $O(n\log n\log V)$。