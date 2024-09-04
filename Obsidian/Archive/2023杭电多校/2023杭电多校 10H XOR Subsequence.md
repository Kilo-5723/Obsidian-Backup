---
link: "[23HDU10H](http://acm.hdu.edu.cn/showproblem.php?pid=7390)"
done: true
tags:
  - hamel-basis
---

我们用 $P(\{a_1,\dots,a_n\})$ 表示 $\{a_1,\dots,a_n\}$ 所有子集异或和的可重集合，则题意即为判断是否存在一组 $a_1,\dots,a_n$ 满足可重集合 $\{0,b_1,\dots,b_{2^n-1}\}=P(\{a_1,\dots,a_n\})$。我们可以证明如果存在满足条件的 $a_1,\dots,a_n$，则将任意 $a_i$ 替换为 $a_i'=a_i\oplus a_j\; (i\neq j)$ 后 $P(\{a_1,\dots,a_i',\dots,a_n\})$ 依然满足条件，这相当于用 $a_i’$ 替换 $P(\{a_1,\dots,a_n\})$ 中的 $a_i\oplus a_j$，用 $a_i'\oplus a_j$ 替换 $P(\{a_1,\dots,a_n\})$ 中的 $a_i$。

因此，如果存在满足条件的 $a_1,\dots,a_n$，则一定存在一组满足条件的 $a_1,\dots,a_n$ 满足 $a_1=\min{b_k}$，因为对于任意一组满足条件的 $a_1,\dots,a_n$，一定能找到一组 $a_{p_1}\oplus\dots\oplus a_{p_k}=\min{b_k}$，则我们可以使用 $a_i'=a_i\oplus a_j$ 将这组 $a_1,\dots,a_n$ 中的 $a_{p_1}$ 一步步替换为 $a_{p_1}\oplus\dots\oplus a_{p_k}=\min{b_k}$。

这样，字典序最小的 $a_1,\dots,a_n$ 自然满足 $a_1=\min{b_k}$。而接下来，我们需要将问题从 $b_1,\dots,b_{2^n-1}$ 的情况归约到 $b_1,\dots,b_{2^{n-1}-1}$ 的情况。我们发现，在确认了 $a_1$ 之后，我们可以将 $b_k$ 两两配对为满足 $b_{x_i}\oplus b_{y_i}=a_1$ 的 $(b_{x_i},b_{y_i})$，如果这一步无法配对则答案一定不存在。以 $1,2,3,4,5,6,7$ 为例，在确定 $a_1=1$ 之后，我们可以将其配对为 $\{2,3\},\{4,5\},\{6,7\}$。

我们用 $S_1\oplus S_2=\{v_1\oplus v_2\}\;(v_1\in S_1,v_2\in S_2)$ 表示两个可重集合 $S_1$, $S_2$ 笛卡尔积的异或和，则我们希望在第 $i$ 轮中得到 $P(\{a_1,\dots,a_i\})\oplus (P(\{a_{i+1},\dots,a_n\})\setminus \{0\})$。我们可以将上面的结论拓展到集合形式，如果答案存在，则最小的 $a_{i+1}$ 是所有集合中的最小值。

令该 $a_{i+1}=x_\min$，其在集合 $P(\{a_1,\dots,a_i\})\oplus x'\;(x'\in P(\{a_{i+1},\dots,a_n\})\setminus \{0\}$ 中。我们已经证明若 $x'\in P(\{a_{i+1},\dots,a_n\})\setminus \{0\}$ 则一定存在一组 $\{a_{i+1},\dots,a_n\}$ 满足 $a_{i+1}=x'$，因此一定存在一组答案 $\{a_1,\dots,a_i,x',\dots,a_n\}$，序列按 $x'$ 合并的结果为 $P(\{a_{i-1},\dots,a_i,x'\})\oplus (P(\{a_{i+2},\dots,a_n\})\setminus \{0\})$。又因为 $x_\min\in P(\{a_1,\dots,a_i\})\oplus x'$，我们有 $P(\{a_{i-1},\dots,a_i,x'\})=P(\{a_{i-1},\dots,a_i,x_\min\})$，因此 $\{a_1,\dots,a_i,x_\min,\dots,a_n\}$ 也是一组答案。

我们可以使用集合中最小的元素来表示每一轮中的集合，并使用线性基辅助判断两个集合 $S_i$, $S_j$ 是否满足 $S_i=S_j\oplus x$，其等价于判断 $x\oplus \min S_i$ 是否在 $\min{S_j}$ 和 $\{a_1,\dots,a_i\}$ 构成的线性基中。