
可以发现，游戏的过程一定是：
1. 小偷移动到一个叶子节点：小偷停留在一个非叶节点，一定不会比其子树下的任何一个叶子好
2. 小偷停留不动，直到警察到达一个叶子节点：如果小偷想要移动到某个叶子节点，那小偷一开始就可以这么做，与警察的行动无关
3. 警察到达一个叶子节点：小偷一定停留在一个叶子节点上且不会移动
4. 当警察到达一个叶子节点后，此时所有其他叶子节点都可达，根据警察所在的叶子节点选择一个新节点移动，回到 1.

其中 1. 包含小偷的选择过程，3. 包含警察的选择过程。我们可以发现，除去第一步，整个过程与非叶节点无关，所以我们下面所讨论的节点都是叶子节点。

我们用 $E(u)$ 代表警察当前在 $u$ 节点时抓住小偷的期望，用 $p(u,v)$ 表示警察在 $u$ 节点时小偷选择 $v$ 节点的概率。那么，因为警察和小偷博弈的过程会产生均衡，所以此时无论小偷移动到哪个节点，警察抓住小偷的期望步数都相同。

因此，对任何一个节点 $u$ 和其他叶子节点 $v_1,\dots,v_n$，我们有:
$$ E(u)=dis(u,v_1)+(1-p(u,v_1))E(v_1)=dis(u,v_2)+(1-p(u,v_2))E(v_2) = \dots$$
看到这个式子，我们可以尝试用迭代求解，也就是根据上一轮的 $E$ 算出下一轮的 $E'$，即：
$$ E'(u)=dis(u,v_1)+(1-p(u,v_1))E(v_1)=dis(u,v_2)+(1-p(u,v_2))E(v_2) = \dots$$
我们发现，因为是迭代过程，所以 $dis(u,v_i)$ 和 $E(v_i)$ 都是常量，未知量只有 $p(u,v_i)$。因此，我们把 $p(u,v_i)$ 设为 $x_i$，则我们可以将上述方程改写为以下形式：$$\begin{cases}
a_i=E(v_i) \\
b_i=dis(u,v_i)+E(v_i) \\
E'(u)=-a_ix_i+b_i=-a_jx_j+b_j=\dots \\
\sum x_i = 1
\end{cases}$$
则我们有：$$\begin{align*}
x_i=\frac{b_i}{a_i}-\frac{E'(u)}{a_i} \\
\sum \frac{b_i}{a_i}- \sum \frac{1}{a_i} \cdot E'(u)=1 \\
E'(u) = \frac{\sum \frac{b_i}{a_i}-1}{\sum \frac{1}{a_i}}
\end{align*}$$
在通过迭代计算完所有叶子节点的期望后，对于题目中给出的警察初始节点 $s$，我们同样有无论小偷选择哪个节点期望步数都相同的结论。因此，我们可以用上面同样的方式计算出 $E(s)$。

I upsolved this problem with iteration method [submission:237083785], but I can't prove why its convergence speed is fast enough. Nevertheless, following is my solution.

First, we have the game procedure:

1. Thief moves to a leaf, cause staying at a non-leaf vertex won't be better than any leaf in it's subtree
2. Thief stays at the leaf until police reaches a leaf. If thief moves to another leaf during the police move, he could have move there in 1.
3. Police chooses another leaf and go directly to it, because he knows that thief's best move is waiting on a leaf
4. After police reaches the leaf, if not caught, all other leaves are accessible to thief. Thief select a vertex based on the leaf police is at and move there, then return to 1.

We can see from the above procedure that non-leaf vertices are irrelevant after the first step, so for the following discussion, vertices are all leaf.

We denote the expected moves that police catches thief starting from leaf $u$ by $E(u)$, denote the probability that thief moves to $v$ if police is at $u$ by $p(u,v)$. Then, thief must distribute $p(u,v)$ in a way that no matter how police changes his strategy, $E(u)$ will not be larger. In this case, no matter how police chooses, $E(u)$ stay the same. Thus for $u$ and every other leaf $v_i$, we have
$$E(u)=dis(u,v_1)+(1-p(u,v_1))E(v_1)=dis(u,v_2)+(1-p(u,v_2))E(v_2) = \dots$$
We try to solve it by iteration method, which means calculate new $E'(u)$ by $E(u)$ in the last round, i.e.
$$E'(u)=dis(u,v_1)+(1-p(u,v_1))E(v_1)=dis(u,v_2)+(1-p(u,v_2))E(v_2) = \dots$$
Then, $dis(u,v_i)$ and $E(v_i)$ are all constants, the only variable is $p(u,v_i)$. Hence, we denote $x_i$ by $p(u,v_i)$, and the above equation can be rewritten as
$$\begin{cases}
a_i=E(v_i) \\
b_i=dis(u,v_i)+E(v_i) \\
E'(u)=-a_ix_i+b_i=-a_jx_j+b_j=\dots \\
\sum x_i = 1
\end{cases}$$
Then we have
$$\begin{align*}
x_i=\frac{b_i}{a_i}-\frac{E'(u)}{a_i} \\
\sum \frac{b_i}{a_i}- \sum \frac{1}{a_i} \cdot E'(u)=1 \\
E'(u) = \frac{\sum \frac{b_i}{a_i}-1}{\sum \frac{1}{a_i}} 
\end{align*}$$
For the first step where police starts from $s$, we have the same conclusion that $E(s)$ stays the same no matter how police moves. Hence, we can calculate $E(s)$ by the same equation above.



Following is my proof that it is a linear convergence when $n$ is considered as constant, and for each iteration the error reduces by a factor of $1+n^{-2}$.

Notations:
- $n$ is the number of vertices.
- $m$ is the number of leaves.
- ${\rm dis}(u,v)$ is the distance between $u$ and $v$.
- $E(u)$ is the expected time for police to catch thief starting from $u$.

First, we have three important conclusions:
1. ${\rm dis}(u,v)\ge 1$
2. $E(u) \ge \max {\rm dis}(u,v)\ge 1$, because thief can always choose to go to the furthest leaf
3. $E(u)\le n^2$, because police can always choose a random leaf, the cost is less than $n$ and the chance is $1/n$.

Then, we revisit the iteration formula
$$x'_u=f(x_{v_1},x_{v_2},\dots) = \frac{\sum {\rm dis}(u,v)/x_v+m-1}{\sum 1/x_v}$$
in geometric form, where we start from $B=(0,m-1)$, each leaf $v$ contributes a vector $\left({\rm dis}(u,v)/x_v,1/x_v\right)$, and the slope of the endpoint $X$ is $x'_u$.

We rewrite $x_v$ in the form of $E(v)/k_v$. It's obvious that we always have $x_v>0$ during the iteration, hence $k_v>0$. We denote the vector $({\rm dis}(u,v)/E(v),1/E(v))$ by $A_v$. Then, the endpoint can be represented as
$$X=B + \sum k_v\cdot A_v$$

And also, we can rewrite the fixed point of the iteration formula as the slope of $P$ being $E(u)$, where
$$P=B+\sum A_v$$
Then, we aim to find both the lower bound and the upper bound of the slope of $X$ to prove its convergence and convergence speed.
### Upper bound of $x'_u$
Because $E(u)\ge \max {\rm dis}(u,v)$ as mentioned before, we have that if current $X\ge E(u)$, then $X$ always increases when $k_v$ decreases. This gives us an upper bound by setting every $k'_v=k_\min$. Formally, we define
$$ X'=B+\sum k_\min\cdot A_v = B+ k_\min\cdot\sum A_v$$
and the slope of $X'$ is an upper bound of $X$.

Then, we calculate the slope of $X'$ by finding out the $y$-coordinate of $OX'$ at $x=\sum 1/E(u)$. There are six important points on the plane:
1. $O=(0,0)$
2. $B=(0,m-1)$
3. $P=B+\sum A_v$, the slope of which is $E(u)$, and its $x$-coordinate is $\sum 1/E(u)$
4. $X'=B+k_\min \sum A_v$, the slope of which is upper bound
5. $Y$, the intersection of line $OX'$ and line $x=\sum 1/E(u)$
6. $Q=\left(\sum 1/E(u),0\right)$, so the slope is $\frac{YQ}{OQ}$

We find that $\triangle X'OB$ and $\triangle X'YP$ are similar triangles, so $PY=(m-1)\cdot (k_\min^{-1}-1)$. Also, we can find a lower bound of $PQ$, where $PQ-m+1=\sum {\rm dis}(u,v)/E(u)\ge \sum 1/n^2=(m-1)/n^2$. Therefore, we have
$$\frac{YQ}{OQ}=\frac{PQ+(m-1)\cdot (k_\min^{-1}-1)}{PQ/E(u)}\le \left(\frac{1+n^{-2}+k_\min^{-1}-1}{1+n^{-2}} \right)E(u)$$
Thus we have $${k'}_u^{-1}-1=\frac{x'_u}{E(u)}-1\le \frac{1}{1+n^{-2}}(k_\min^{-1}-1) $$
### Lower bound of $x'_u$
If the slope of $X'$ is less than $E(u)$, then there must be some $k_v>1$, i.e. some of $A_v$ are extended. The lower bound of slope of $X'$ is that we suppose all extended $A_v$ has the minimum possible slope, i.e. ${\rm dis}(u,v)=1$. This gives us a lower bound by setting every $k_v'=k_\max$ and the slope of extended part is $1$. Formally, we define
$$ X'=P+p\cdot (k_\max-1,k_\max-1)$$
and the slope of $X'$ is a lower bound of $X$.

Here we denote the $x$-coordinate of $P$ by $p=\sum 1/E(v)$. Then, we calculate the slope of $X'$ directly, we have the slope of $X'$ being
$$\frac{p\cdot E(u)+p\cdot (k_\max-1)}{p+p\cdot (k_\max-1)}=\frac{E(u)+k_\max-1}{k_\max}$$
Thus we have
$$1-{k'_u}^{-1}=1-\frac{x'_u}{E(u)}=1-k_\max^{-1}-E^{-1}(u)+k_\max^{-1}\cdot E^{-1}(u)-1 = (1-k_\max^{-1})\cdot (1-E^{-1}(u))$$
Where $E(u)\le n^2$, therefore
$$1-{k'_u}^{-1}\le \left(1-n^{-2}\right)(1-k_\max^{-1})$$
Hence both $k_\min^{-1}-1$ and $1-k_\max^{-1}$ reduce by a factor of $1+O\left(n^{-2}\right)$ for each iteration, and the convergence is linear if $n$ is considered a constant.