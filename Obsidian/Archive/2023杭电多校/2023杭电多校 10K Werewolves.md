---
link: "[23HDU10K](http://acm.hdu.edu.cn/showproblem.php?pid=7393)"
done: true
tags:
  - construction
---

考虑将 $n$ 个人均分为 $m$ 组，使每种情况下都有一组人能猜对，则可以让第 $i$ 组人猜所有玩家身份牌数字之和 $\equiv i \pmod m$，可以发现这种构造满足题目要求。