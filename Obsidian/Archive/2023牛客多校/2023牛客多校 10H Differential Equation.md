---
Link: "[23NC10H](https://ac.nowcoder.com/acm/contest/57364/H)"
done: true
tags:
  - calculus
  - generating-function
  - polynomial
---

$F_k(x)=\left(x^2-1\right)F'_{k-1}(x)$ 的条件太过复杂，因此我们尝试使用换元法将 $F_k(x)$ 换元为 $G_k(u)$，其中 $G_k(u)=F_k(x)$ 且 $G_k(u)=G'_{k-1}(u)$，即满足 $G_k(u)=G_0^{(k)}(u)$。我们有：
$$\begin{align}
F_k(x)&=\left(x^2-1\right)F'_{k-1}(x) \\
G_k(u)&=\left(x^2-1\right)\frac{\mathrm{d} G_{k-1}(u)}{\mathrm{d} x} \\
G_k(u)&=\left(x^2-1\right)\frac{\mathrm{d} G_{k-1}(u)}{\mathrm{d} u}\cdot \frac{\mathrm{d} u}{\mathrm{d} x}\\
G_k(u)&=\left(x^2-1\right) G'_{k-1}(u)\frac{\mathrm{d} u}{\mathrm{d} x}\\
\end{align}$$
因为 $G_k(u)=G'_{k-1}(u)$，我们有：
$$\begin{align}
1&=\left(x^2-1\right)\frac{\mathrm{d} u}{\mathrm{d} x}\\
\frac{\mathrm{d} u}{\mathrm{d} x}&=\frac{1}{x^2-1}\\
\frac{\mathrm{d} u}{\mathrm{d} x}&=\frac{1}{(x-1)(x+1)}\\
\frac{\mathrm{d} u}{\mathrm{d} x}&=\frac{1}{2}\left(\frac{1}{x-1}-\frac{1}{x+1}\right)\\
\end{align}$$
我们可以构造 $u=\left(\ln(x-1)-\ln(x+1)\right)/2$。另外，根据 $F_0(x)=x$ 我们有 $G_0(u)=x$，其中：
$$\begin{align}
u&=\frac{\ln(x-1)-\ln(x+1)}{2} \\
e^{2u}&=\frac{x-1}{x+1} \\
e^{2u}-1&=-\frac{2}{x+1} \\
\frac{2}{1-e^{2u}}&=x+1 \\
\frac{1+e^{2u}}{1-e^{2u}}&=x=G_0(u) \\
\end{align}$$
因此，我们有 $\displaystyle G_0(u)=\frac{1+e^{2u}}{1-e^{2u}}$。$G_0(u+z)$ 在 $u$ 的泰勒展开式如下：
$$
\frac{1+e^{2(u+z)}}{1-e^{2(u+z)}}=\displaystyle G_0(u+z)=\sum\limits_{n=0}^{\infty}\frac{G_n(u)}{n!}z^n
$$
当 $x=x_0$，即 $u=u_0=\left(\ln(x_0-1)-\ln(x_0+1)\right)/2$ 时，我们有：
$$
\sum\limits_{n=0}^{\infty}\frac{F_n(x_0)}{n!}z^n=\sum\limits_{n=0}^{\infty}\frac{G_n(u)}{n!}z^n\frac{1+e^{2(u+z)}}{1-e^{2(u+z)}} = \frac{1+e^{\ln(x_0-1)-\ln(x_0+1)+2z}}{1-e^{\ln(x_0-1)-\ln(x_0+1)+2z}}=\frac{x_0+1+(x_0-1)e^{2z}}{x_0+1-(x_0-1)e^{2z}}
$$
因此，$\displaystyle\sum\limits_{n=0}^{\infty}\frac{F_n(x_0)}{n!}z^n$ 是 $\displaystyle\frac{x_0+1+(x_0-1)e^{2z}}{x_0+1-(x_0-1)e^{2z}}$ 的指数生成函数，我们可以通过多项式运算得到这个函数的生成函数，时间复杂度为 $O(n\log n)$。