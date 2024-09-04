---
dg-publish: true
source: Codeforces
contest: Educational Codeforces Round 147
cid: "1821"
problem: F
name: Timber
tags:
  - todo
  - generating-function
lead:
---
```dataviewjs
const blog = require(app.vault.adapter.getBasePath()+"/.script/blog.js");
blog.problem_header(dv);
```

### 题意简述

|  记号 | 含义  |
| --: | :-- |
|     |     |

### 题解

$$ \begin{align*}

f(x) & = 0x^0 + 0x^1 + \dots + 0x^k + 2x^{k+1} + 2x^{k+2} + \dots + 2x^{2k} + x^{2k+1} + x^{2k+2} + \dots \\

& = 2x^{k+1}\cdot \frac{1}{1-x} - x^{2k+1}\cdot \frac{1}{1-x} \\

& = \frac{-x^{2k+1}+2x^{k+1}}{1-x} \\

\end{align*}

$$

  

$$F(x) = f^m(x) = \left(\frac{-x^{2k+1}+2x^{k+1}}{1-x}\right)^m$$

  

$$

\begin{align*}

F'(x) &= m\cdot\left(\frac{-x^{2k+1}+2x^{k+1}}{1-x}\right)^{m-1}\cdot\frac{\left(-(2k+1)x^{2k}+(2k+2)x^k\right)(1-x)-x^{2k+1}+2x^{k+1}}{(1-x)^2} \\

&= m\cdot F(x)\cdot\frac{(2k+1)x^{2k+1}-(2k+1)x^{2k}-(2k+2)x^{k+1}+(2k+2)x^k-x^{2k+1}+2x^{k+1}}{(1-x)(-x^{2k+1}+2x^{k+1})} \\

&= m\cdot F(x)\cdot\frac{2kx^{2k+1}-(2k+1)x^{2k}-2kx^{k+1}+(2k+2)x^k}{x^{2k+2}-x^{2k+1}-2x^{k+2}+2x^{k+1}} \\

&= m\cdot F(x)\cdot\frac{2kx^{k+1}-(2k+1)x^{k}-2kx+2k+2}{x^{k+2}-x^{k+1}-2x^2+2x}

\end{align*}

$$

  

$$

\begin{align*}

F'(x) \cdot\left(x^{k+2}-x^{k+1}-2x^2+2x\right) &= m\cdot F(x)\cdot \left(2kx^{k+1}-(2k+1)x^{k}-2kx+2k+2\right) \\

\sum\limits_{i=1}^{\infty}ia_ix^{i-1}\left(x^{k+2}-x^{k+1}-2x^2+2x\right) &= \sum\limits_{i=0}^\infty ma_ix^i\left(2kx^{k+1}-(2k+1)x^{k}-2kx+2k+2\right) \\

\sum\limits_{i=1}^{\infty}(ia_i)x^{i+k+1}-(ia_i)x^{i+k}-(2ia_i)x^{i+1}+(2ia_i)x^i &= \sum\limits_{i=0}^\infty (2kma_i)x^{i+k+1}-((2k+1)ma_i)x^{i+k}-(2kma_i)x^{i+1}+(2k+2)ma_ix^i \\

\end{align*}

$$

  

$$

\begin{align*}

\sum\limits_{i=0}^{\infty}(2km-i)a_ix^{i+k+1}-(2km+m-i)a_ix^{i+k}-(2km-2i)a_ix^{i+1}+(2km+2m-2i)a_ix^i = 0 \\

\end{align*}

$$

  

$$

(2km-(n-k-1))a_{n-k-1} - (2km+m-(n-k))a_{n-k}-(2km-2(n-1))a_{n-1}+(2km+2m-2n)a_n = 0

$$



$$

a_n = -\frac{(2km-(n-k-1))a_{n-k-1} - (2km+m-(n-k))a_{n-k}-(2km-2(n-1))a_{n-1}}{2km+2m-2n}

$$