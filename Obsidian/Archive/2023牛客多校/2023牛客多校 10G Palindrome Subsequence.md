---
Link: "[23NC10G](https://ac.nowcoder.com/acm/contest/57364/G)"
done: true
tags:
  - string
  - greedy
  - dynamic-programming
---

我们在本文中使用下列记号：
- 对于给定的回文串 $s$，我们定义 $s$ 的前半段为 $\text{half}(s)=s_1\dots s_{\lceil |s|/2\rceil}$。
- 对于给定的字符串 $s$，我们定义 $s$ 的反串为 $\text{rev}(s)=s_{|s|}\dots s_1$。
- 我们将输入字符串第 $i$ 位后（含）的第一个固定点表示为 $\text{nextfx}(i)$，不存在则为 $|s|+1$。
- 我们将输入字符串第 $i$ 位前（含）的最后一个固定点表示为 $\text{prevfx}(i)$，不存在则为 $0$。
- 对于给定的字符 $c$，我们将输入字符串第 $i$ 位后（含）$c$ 第一次出现的位置表示为 $\text{nextch}(i,c)$，不存在则为 $|s|+1$。
- 对于给定的字符 $c$，我们将输入字符串第 $i$ 位前（含）$c$ 最后一次出现的位置表示为 $\text{prevch}(i,c)$，不存在则为 $0$。
- 我们定义 $s_l\dots s_r$ 的合法子序列为满足固定点限制的子序列。
- 我们定义 $\text{valid}(l,r)$ 为 $s_l\dots s_r$ 中是否存在一个合法回文子序列。
- 我们定义 $\text{maxch}(l,r)$ 为 $s_l\dots s_r$ 中合法回文子序列的最大首字母。

求出字典序最大的合法字符串的常见方法是贪心，即在每一轮迭代时向当前的答案末尾加入一个尽可能大的字符，使得加入该字符后答案依然合法。然而，在这个问题中我们需要找到一个字典序最大的回文串。因此贪心的策略有所不同。

令合法回文串集为 $V$，我们令 $s_{ans}=\max_{s\in V} s$，$s_{max}=\arg\max_{s\in V}\text{half}(s)$，即 $s_{ans}$ 是字典序最大的合法回文串，$s_{max}$ 是前半段字典序最大的合法回文串。我们可以找到 $s_{ans}\neq s_{max}$ 的例子，例如 $s_{ans}=\texttt{"bb"}$, $s_{max}=\texttt{"bab"}$，但 $\text{half}(s_{ans})$ 一定是 $\text{half}(s_{max})$ 的前缀，否则 $s_{ans}$ 的字典序将比 $s_{max}$ 小。因此，我们可以如下设计贪心算法：
1. 将 $s_{cur}$ 和 $s_{ans}$ 初始化为 $\texttt{""}$。
2. 找到令 $s_{cur}+x+\text{rev}(s_{cur})$ 合法的首字母最大的回文串 $x$，将其首字母记为 $c$。
3. 查询 $s+c+\text{rev}(s)$ 和 $s+c+c+\text{rev}(s)$ 是否合法，如果合法则更新 $s_{ans}$。
4. 令 $s\gets s+c$ 并返回步骤 2，直到没有任何 $c$ 可以被加到 $s$ 的末尾。

为了实现这个贪心算法，我们需要高效地实现步骤 2。因此，我们维护一个区间 $(l,r)$ 的集合来表示当前所有可能的 $x$ 所在的区间，其中 $s_{cur}$ 和 $\text{rev}(s_{cur})$ 分别是 $s_1\dots s_{l-1}$ 和 $s_{r+1}\dots s_{|s|}$ 的合法子序列。我们可以设计如下的贪心算法：
1. 将 $s_{cur}$ 和 $s_{ans}$ 初始化为 $\texttt{""}$，将 $Q$ 初始化为 $\{(1,n)\}$。
2. 找到 $(l,r)\in Q$ 中最大的 $\text{maxch}(l,r)$，记为 $c$。
3. 查询 $c$ 和 $cc$ 是否为 $(l,r)\in Q$ 的合法子序列，如果是则更新 $s_{ans}$。
4. 对 $(l,r)\in Q$，找到所有区间 $(l',r')$，其满足 $\text{valid}(l',r')$ 且 $c$ 是 $s_l\dots s_{l'-1}$ 和 $s_{r'+1}\dots s_r$ 合法子序列，将 $(l',r')$ 加入 $Q'$。
5. 令 $s\gets s+c$, $Q\gets Q'$ 并返回步骤 2，直到没有任何 $c$ 可以被加到 $s$ 的末尾。

要实现这个算法，我们需要解决下列子问题：
1. 对于给定的区间 $(l,r)$，求出 $\text{valid}(l,r)$.
2. 对于给定的区间 $(l,r)$，求出 $\text{maxch}(l,r)$.
3. 对于给定的区间 $(l,r)$ 和一个字符 $c$，找到所有区间 $(l',r')$，其满足 $\text{valid}(l',r')$ 且 $c$ 是 $s_l\dots s_{l'-1}$ 和 $s_{r'+1}\dots s_r$ 合法子序列。
令 $l'=\text{nextfx}(l)$，$r'=\text{prevfx}(r)$，我们依次解决每个子问题。

首先，要求出 $\text{valid}(l,r)$，有以下几种情况：
- $l'>r$ 且 $r'<l$，即 $s_l\dots s_r$ 不包含固定点。$\texttt{""}$ 此时就是 $s_l\dots s_r$ 的一个合法子序列。
- $\text{prevch}(r,s_{l'})>r'$ 且 $\text{valid}(l'+1,\text{prevch}(r,s_{l'})-1)$。
- $\text{nextch}(l,s_{r'})<l$ 且 $\text{valid}(\text{nextch}(l,s_{r'})+1,r'-1)$。
- $s_{l'}=s_{r'}$ 且 $\text{valid}(l'+1,r'-1)$。
如果上述所有条件都不成立，则 $\text{valid}(l,r)$ 不成立。求出所有 $\text{valid}(l,r)$ 的时间复杂度为 $O(n^2)$。

接下来，要求出 $\text{maxch}(l,r)$，相似地，存在以下几种情况：
- $s_{l^*}=s_{r^*}$，其中 $l^*\in [l,l')$，$r^*\in (r',r]$ 且 $\text{valid}(l^*+1,r^*-1)$。$s_{l^*}$ 有可能是 $\text{maxch}(l,r)$。这包含了 $l'>r$ 且 $r'<l$ 的情况。
- $s_{l^*}=s_{r'}$，其中 $l^*\in [l,l')$ 且 $\text{valid}(l^*+1,r'-1)$。$s_{l^*}$ 有可能是 $\text{maxch}(l,r)$。
- $s_{l'}=s_{r^*}$，其中 $r^*\in (r',r]$ 且 $\text{valid}(l'+1,r^*-1)$。 $s_{r^*}$ 有可能是 $\text{maxch}(l,r)$。
- $s_{l'}=s_{r'}$。$s_{l'}$ 有可能是 $\text{maxch}(l,r)$。
求出所有 $\text{match}(l,r)$ 的时间复杂度为 $O(n^2|C|)$，其中 $C$ 是字符集。

最后，要求出所有满足 $\text{valid}(l',r')$ 且 $c$ 是 $s_l\dots s_{l'-1}$ 和 $s_{r'+1}\dots s_r$ 合法子序列的区间 $(l',r')$，其可能的情况与求 $\text{maxch}(l,r)$ 时完全相同，但我们只将最小的 $l^*$ 和最大的 $r^*$ 加入集合，因为更大的 $l^*$ 和更小的 $r^*$ 不会让后续的选择更优。

每一轮迭代中 $|Q|\le n(n+1)/2$，而因为答案的长度最多为 $n$，迭代轮数最多为 $\lceil n/2\rceil$。因此，总时间复杂度为 $O(n^3+n^2|C|)$。