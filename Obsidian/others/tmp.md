Following is my proof that it is a linear convergence when $n$ is considered as constant, and for each iteration the error reduces by a factor of $1+n^{-2}$. Please ignore dollar signs in the front of some lines, they are for preventing [this bug](https://codeforces.com/blog/entry/83174).

Notations:

- $n$ is the number of vertices.

- $m$ is the number of leaves.

- ${\rm dis}(u,v)$ is the distance between $u$ and $v$.

- $E(u)$ is the expected time for police to catch thief starting from $u$.

First, we have three important conclusions:

1. ${\rm dis}(u,v)\ge 1$

2. $E(u) \ge \max {\rm dis}(u,v)$, because thief can always choose to go to the furthest leaf

3. $E(u)\le n^2$, because police can always choose a random leaf, the cost is less than $n$ and the chance is greater than $1/n$.

Then, we revisit the iteration formula
$$x'_u=\frac{\sum {\rm dis}(u,v)/x_v+m-1}{\sum 1/x_v}$$
$ in geometric form, where we start from $B=(0,m-1)$, each leaf $v$ contributes a vector $\left(1/x_v,{\rm dis}(u,v)/x_v\right)$, and the slope of the endpoint $X$ is $x'_u$, where
$$X=B+\sum \left(1/x_v,{\rm dis}(u,v)/x_v\right)$$
$ We rewrite $x_v$ in the form of $E(v)/k_v$, then $X$ can be represented as 
$$X=B+\sum k_v\cdot \left(1/E(v),{\rm dis}(u,v)/E(v)\right)$$
$ It's obvious that we always have $x_v>0$ during the iteration, hence $k_v>0$. We denote the vector $(1/E(v),{\rm dis}(u,v)/E(v))$ by $\overrightarrow{a_v}$. Then, the endpoint can be represented as
$$X=B + \sum k_v\cdot \overrightarrow{a_v}$$

$ And also, at the fixed point we have each $x_v=E(v)$ and hence all $k_v=1$. Therefore, we can rewrite the fixed point of the iteration formula as the slope of $P$ being $E(u)$, where
$$P=B+\sum \overrightarrow{a_v}$$
$ Then, we aim to find both the lower bound and the upper bound of the slope of $X$ to prove its convergence and convergence speed.
### Upper bound of $x'_u$
Because $E(u)\ge \max {\rm dis}(u,v)$ as mentioned before, we have that the slope of $P$ is larger than any slope of $\overrightarrow{a_v}$. Therefore, if current $X\ge E(u)$, then $X$ always increases when $k_v$ decreases. This gives us an upper bound by setting every $k'_v=k_\min=\min\{k_v\}$. Formally, we define
$$ X'=B+\sum k_\min\cdot\overrightarrow{a_v} = B+ k_\min\cdot\sum \overrightarrow{a_v}$$
and the slope of $X'$ is an upper bound of $X$.

We denote the $x$-coordinate of $P$ by $p_x=\sum 1/E(v)$. Then, we calculate the slope of $X'$ by finding out the $y$-coordinate of $OX'$ at $x=p_x$. There are six important points on the plane:

1. $O=(0,0)$

2. $B=(0,m-1)$

3. $P=B+\sum \overrightarrow{a_v}$, the slope of which is $E(u)$

4. $X'=B+k_\min \sum \overrightarrow{a_v}$, the slope of which is the upper bound

5. $Y$, the intersection of line $OX'$ and line $x=p_x$

6. $Q=\left(p_x,0\right)$, so the slope of $X'$ is $\frac{YQ}{OQ}$

We find that $\triangle X'OB$ and $\triangle X'YP$ are similar triangles, so $PY=(m-1)\cdot (k_\min^{-1}-1)$. Also, we can find a lower bound of $PQ$, where
$$PQ=m-1+\sum {\rm dis}(u,v)/E(u)\ge m-1+\sum 1/n^2=(m-1)\left(1+n^{-2}\right)$$
Therefore, we have
$$x'_u=\frac{YQ}{OQ}=\frac{PQ+(m-1)\cdot (k_\min^{-1}-1)}{PQ/E(u)}\le \frac{1+n^{-2}+k_\min^{-1}-1}{1+n^{-2}} E(u)$$
Thus we have $${k'}_u^{-1}-1=\frac{x'_u}{E(u)}-1\le \frac{1}{1+n^{-2}}(k_\min^{-1}-1) $$
### $ Lower bound of $x'_u$
If the slope of $X'$ is less than $E(u)$, then there must be some $k_v>1$, i.e. some of $\overrightarrow{a_v}$ are extended. The lower bound of slope of $X'$ is that we suppose all extended $\overrightarrow{a_v}$ has the minimum possible slope, i.e. ${\rm dis}(u,v)=1$. This gives us a lower bound by setting every $k_v'=k_\max=\max\{k_v\}$ and the slope of extended part is $1$. Formally, we define
$$ X'=P+p\cdot (k_\max-1,k_\max-1)$$
and the slope of $X'$ is a lower bound of $X$.

Here we also denote the $x$-coordinate of $P$ by $p_x=\sum 1/E(v)$. Then, we calculate the slope of $X'$ directly, we have the slope of $X'$ being
$$x'_u=\frac{p_x\cdot E(u)+p_x\cdot (k_\max-1)}{p_x+p_x\cdot (k_\max-1)}=\frac{E(u)+k_\max-1}{k_\max}$$
Thus we have
$$1-{k'_u}^{-1}=1-\frac{x'_u}{E(u)}=1-k_\max^{-1}-E^{-1}(u)+k_\max^{-1}\cdot E^{-1}(u) = (1-k_\max^{-1})\cdot (1-E^{-1}(u))$$
$ Where $E(u)\le n^2$, therefore
$$1-{k'_u}^{-1}\le \left(1-n^{-2}\right)(1-k_\max^{-1})$$
$ Hence both $k_\min^{-1}-1$ and $1-k_\max^{-1}$ reduce by a factor of $1+n^{-2}$ for each iteration, and the convergence is linear if $n$ is considered a constant.