---
dg-publish: true
dg-home: true
---

Welcome back.

# 博文列表

```dataview
table choice(done,"★"," ") as Done
from "Notes"
where dg-publish = true
sort done desc
```

## 博文更新计划

- Dijkstra 费用流
- 积性函数前缀和
- Nimber
- 三角剖分

# 一天一题目，学位远离我
## A problem a day keeps the doctorate away

### Codeforces 3000+

```dataviewjs
const main = require(app.vault.adapter.getBasePath()+"/.script/table.js");
var a = [];
var list = dv.pages('"Solution"').values.filter(a => { return a.source == "Codeforces" })
for (let i in list) await main.add_dynamic_property(list[i]);
list = list
  .filter((a) => { return a.rating >= 3000;})
  .sort((a, b) => { return (a.cid > b.cid) - (a.cid < b.cid)})
  .sort((a, b) => { return (a.problem > b.problem)
    - (a.problem < b.problem)})
  .sort((a, b) => { return (a.rating < b.rating) - (a.rating > b.rating); })
for (let i in list) a.push([main.to_item(dv,list[i])]);
dv.table([], a);
```

### AtCoder 3200+

```dataviewjs
const main = require(app.vault.adapter.getBasePath()+"/.script/table.js");
var a = [];
var list = dv.pages('"Solution"').values.filter(a => { return a.source == "AtCoder" })
for (let i in list) await main.add_dynamic_property(list[i]);
list = list
  .filter((a) => { return a.rating >= 3200; })
  .sort((a, b) => { return (a.cid > b.cid) - (a.cid < b.cid)})
  .sort((a, b) => { return (a.problem > b.problem)
    - (a.problem < b.problem)})
  .sort((a, b) => { return (a.rating < b.rating) - (a.rating > b.rating); })
for (let i in list) a.push([main.to_item(dv, list[i])]);
dv.table([], a);
```

### Full List

```dataviewjs
const main = require(app.vault.adapter.getBasePath()+"/.script/table.js");
var a = [];
var list = dv.pages('"Solution"').values
  .filter(a => { return a["dg-publish"]; });
for (let i in list) await main.add_dynamic_property(list[i]);
list = list.sort((a, b) => { return (a.problem > b.problem)
    - (a.problem < b.problem); })
  .sort((a, b) => { return (a.cid < b.cid) - (a.cid > b.cid); })
  .sort((a, b) => { return (a.source > b.source) - (a.source < b.source) });
//  .sort((a, b) => {
//  var pa = 0, pb = 0;
//  if (a.draft) pa++;
//  if (a.todo) pa+=2;
//  if (b.draft) pb++;
//  if (b.todo) pb+=2;
//  return (pa > pb) - (pa < pb) });
for (let i in list) a.push([main.to_item(dv,list[i])]);
dv.table([], a);
```

### Speedrun

```dataview
table clist as "Clist", speedrun as "Speedrun"
from "Speedrun"
where dg-publish
sort clist DESC
```

##### 夜店主题好劲啊！

首页不知道放什么，就把之前翻译的英文版知识点丢到后面了。

# ICPC Knowledge List (Translated from Chinese)

Original Authors: Yuhao Du, Mr. Wo

## Colors / Marks

<span style = "background-color:Aqua;color:black">&nbsp;Basic, D&nbsp;</span>  <span style = "background-color:Lime;color:black">&nbsp;Bronze, C&nbsp;</span>  <span style = "background-color:SkyBlue;color:black">&nbsp;Silver, B&nbsp;</span>   <span style = "background-color:Orange;color:black">&nbsp;Gold, A&nbsp;</span>   <span style = "background-color:Red;color:black">&nbsp;Champion, S&nbsp;</span>  <span style = "background-color:Black;color:white">&nbsp;Template, T&nbsp;</span>  <span style = "background-color:HotPink;color:black">&nbsp;Useful, Y&nbsp;</span>  <span style = "background-color:Silver;color:black">&nbsp;Useless, N&nbsp;</span>

### Basic
  - Language Basic
    - <span style = "background-color:Aqua;color:black">&nbsp;Function, D&nbsp;</span>
    - <span style = "background-color:Aqua;color:black">&nbsp;Pointer, D&nbsp;</span>
    - <span style = "background-color:Aqua;color:black">&nbsp;Reference, D&nbsp;</span>
    - <span style = "background-color:Aqua;color:black">&nbsp;Struct / Class, D&nbsp;</span>
  - Algorithm Basic
    - Sorting
      - <span style = "background-color:Aqua;color:black">&nbsp;Selection Sort, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Bubble Sort, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Insertion Sort, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Quick Sort, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Merge Sort, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Bucket Sort, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Radix Sort, D&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Heap Sort, N&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Shell Sort, N&nbsp;</span>
  - <span style = "background-color:Aqua;color:black">&nbsp;Big Decimal, D&nbsp;</span>
  - <span style = "background-color:Aqua;color:black">&nbsp;Bit Operation, D&nbsp;</span>
  - <span style = "background-color:Aqua;color:black">&nbsp;Complexity Analysis, D&nbsp;</span>

- Dynamic Programming
  - <span style = "background-color:Lime;color:black">&nbsp;Memorization, C&nbsp;</span>
  - Basic
    - <span style = "background-color:Lime;color:black">&nbsp;Optimal Substructure, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Non-Aftereffect Property, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;State, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;State-Transition Equation, C&nbsp;</span>
  - Knapsack Problem
    - <span style = "background-color:Lime;color:black">&nbsp;0-1 Knapsack Problem, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Unbounded Knapsack Problem, C&nbsp;</span>
    - Multiple Knapsack Problem
      - <span style = "background-color:Lime;color:black">&nbsp;$O(nml)$, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;$O(nm\log l)$, C&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;$O(n^2)$, B&nbsp;</span>
    - Tree Knapsack Problem
      - <span style = "background-color:Lime;color:black">&nbsp;$O(n^3)$, C&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;$O(n^2)$, B&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Group Knapsack Problem, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Mixed Knapsack Problem, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Multi-Dimensional Knapsack Problem, C&nbsp;</span>
  - Range DP
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Advanced, B&nbsp;</span>
  - Digit DP
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Advanced, B&nbsp;</span>
  - DP on Tree (Basic, Re-Rooting)
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Advanced, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Senior, A&nbsp;</span>
  - Bitmask DP
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;DP on Broken Profile, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;High Dimentional Prefix Sum, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Dynamic High Dimentional Prefix Sum, A&nbsp;</span>
    - <span style = "background-color:Red;color:black">&nbsp;Plug DP, S&nbsp;</span>
  - Probabilistic DP
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Advanced, B&nbsp;</span>
  - Counting by DP
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Advanced, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Senior, A&nbsp;</span>
  - Others
    - <span style = "background-color:SkyBlue;color:black">&nbsp;DP on Graph, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;DP on Cycpe Tree, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;DP on Automaton, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;DP with Divide and Counquer, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Filling DP, A&nbsp;</span>
    - <span style = "background-color:Red;color:black">&nbsp;Dynamic DP, S&nbsp;</span>
    - <span style = "background-color:Red;color:black">&nbsp;DP on DP, S&nbsp;</span>
  - Optimization
    - <span style = "background-color:SkyBlue;color:black">&nbsp;by Monotonic Stack, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;by Monotonic Queue, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;by Quadrangle Inequality, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;by Slope, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;by Data Structure, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;by Monotone Decision, A&nbsp;</span>

### Data Structure
  - Queue
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Double-Ended Queue, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Monotonic Queue, C&nbsp;</span>
  - Stack
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Monotonic Stack, C&nbsp;</span>
  - Linked List
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Doubly Linked List, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Circular Linked List, C&nbsp;</span>
  - Heap
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Two Heaps, C&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Fibonacci Heap, N&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Leftlist Heap, N&nbsp;</span>
  - Tree
    - Basic
      - <span style = "background-color:Lime;color:black">&nbsp;Basic Concept, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Traversal, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Centroid, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Diameter, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Lowest Common Ancestor, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;DFN Order, C&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Bracket Order, B&nbsp;</span>
    - Segment Tree
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Basic, B&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Lazy Propagation, B&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Persistent Segment Tree, A&nbsp;</span>
      - <span style = "background-color:Red;color:black">&nbsp;Li-Chao Segment Tree, S&nbsp;</span>
      - <span style = "background-color:Red;color:black">&nbsp;Merging Segment Tree, S&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;ZKW Segment Tree, N&nbsp;</span>
    - Binary Array
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Basic, B&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;K-th Largest Query, B&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Multi-Dimensional Binary Array, B&nbsp;</span>
    - Balanced Binary Search Tree
      - <span style = "background-color:Orange;color:black">&nbsp;Splay, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Treap, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;FHQ Treap, A&nbsp;</span>
      - <span style = "background-color:Red;color:black">&nbsp;Persistent Balanced Binary Search Tree, S&nbsp;</span>
      - Others
        - <span style = "background-color:Red;color:black">&nbsp;Scapegoat Tree, S&nbsp;</span>
        - <span style = "background-color:Silver;color:black">&nbsp;AVL Tree, N&nbsp;</span>
        - <span style = "background-color:Silver;color:black">&nbsp;Size Balanced Tree, N&nbsp;</span>
        - <span style = "background-color:Silver;color:black">&nbsp;Red-Black Tree, N&nbsp;</span>
    - Tree of Tree
      - <span style = "background-color:Orange;color:black">&nbsp;Segment Tree of Segment Tree, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Segment Tree of Balanced Binary Search Tree, A&nbsp;</span>
    - Tree Decomposition
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Heavy Path Decomposition, B&nbsp;</span>
      - <span style = "background-color:Red;color:black">&nbsp;Longest Path Decomposition, S&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Cartesian Tree, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;DSU on Tree, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Link-Cut Tree, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Virtual Tree, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;K-Dimentional Tree, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Tree Hashing, A&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Tree of List, N&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Partition Tree, N&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;And-Or Tree, T&nbsp;</span>
  - Disjoint Set Union
    - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Weighted Disjoint Set Union, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Persistent Disjoint Set Union, A&nbsp;</span>
  - Square Root Decomposition
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Mo's algorithm, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Square Root Decomposition on Chain, B&nbsp;</span>
    - <span style = "background-color:Red;color:black">&nbsp;Square Root Decomposition on Tree, S&nbsp;</span>
  - Standard Template Library
    - <span style = "background-color:Lime;color:black">&nbsp;pair, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;vector, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;priority_queue, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;set, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;map, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;multiset, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;bitset, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;pbds, A&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;unordered_set, N&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;unordered_map, N&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Sparse Table, C&nbsp;</span>
  - <span style = "background-color:Silver;color:black">&nbsp;Dancing Links, N&nbsp;</span>

### Graph
  - <span style = "background-color:Lime;color:black">&nbsp;Basic Concept, C&nbsp;</span>
  - Traversal
    - <span style = "background-color:Lime;color:black">&nbsp;Depth First Search, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Breadth First Search, C&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Topological Order, C&nbsp;</span>
  - Transitive Closure
    - <span style = "background-color:Lime;color:black">&nbsp;Floyd–Warshall Algorithm, C&nbsp;</span>
  - Shortest Path
    - <span style = "background-color:Lime;color:black">&nbsp;Bellman–Ford Algorithm, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Dijkstra's Algorithm, C&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Shortest Path Tree, A&nbsp;</span>
  - Minimum Spanning Tree
    - <span style = "background-color:Lime;color:black">&nbsp;Prim's Algorithm, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Kruskal's Algorithm, C&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Borůvka's Algorithm, A&nbsp;</span>
  - Biconnected Component and 2-Edge-Connected Component
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Tarjan's Strongly Connected Components Algorithm, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Cut Vertex and Bridge, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Block-Cut Tree, A&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Kosaraju's Algorithm, T&nbsp;</span>
  - Bipartite Graph
    - <span style = "background-color:Lime;color:black">&nbsp;Bipartite Graph Check, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Hungarian Algorithm, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Kuhn-Munkres Algorithm, A&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Hopcroft-Karp Algorithm, T&nbsp;</span>
  - Network Flow
    - Maximum Flow
      - <span style = "background-color:Orange;color:black">&nbsp;Dinic, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Shortest Augmenting Path Algorithm, A&nbsp;</span>
    - Minimum-Cost Flow
      - <span style = "background-color:Orange;color:black">&nbsp;Minimum Cost Maximum Flow, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Minimum Cost Feasible Flow, A&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;ZKW Cost Flow, N&nbsp;</span>
    - <span style = "background-color:Red;color:black">&nbsp;Flow with Lower Bound, S&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Euler Tour Technique, C&nbsp;</span>
  - <span style = "background-color:SkyBlue;color:black">&nbsp;2-Satisfiability, B&nbsp;</span>
  - <span style = "background-color:SkyBlue;color:black">&nbsp;Tournament Graph, B&nbsp;</span>
  - 

### Geometry
  - Vector
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Dot Product, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Cross Product, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Angular Coordinate, B&nbsp;</span>
  - Segment
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Intersection Check, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Intersection, B&nbsp;</span>
  - Polygon
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Convex Hull, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Polygon Inclusion, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Rotating Calipers, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Half-Plane Intersection, A&nbsp;</span>
  - Circle
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Intersection / Tangent, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Area of Cross / Union, A&nbsp;</span>
  - <span style = "background-color:Red;color:black">&nbsp;Fast Operation on Convex Hull, S&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;3-D Geometry, T&nbsp;</span>
  - Others
    - Numerical Integration
      - <span style = "background-color:Orange;color:black">&nbsp;Simpson's Rule, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Adaptive Simpson's Rule, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Percise Point Positioning, A&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Minimum Covering Circle, T&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Voronoi Diagram, T&nbsp;</span>
  - <span style = "background-color:Silver;color:black">&nbsp;Inversion Transformation of Circle, N&nbsp;</span>
  - <span style = "background-color:Silver;color:black">&nbsp;Pick's Theorem, N&nbsp;</span>
  - <span style = "background-color:SkyBlue;color:black">&nbsp;Difference Constraints, B&nbsp;</span>
  - <span style = "background-color:Orange;color:black">&nbsp;Steiner Tree, A&nbsp;</span>
  - <span style = "background-color:Orange;color:black">&nbsp;Cactus Graph, A&nbsp;</span>
  - <span style = "background-color:Red;color:black">&nbsp;Directed Minimum Spanning Tree, S&nbsp;</span>
  - <span style = "background-color:Red;color:black">&nbsp;Maximum Cardinality Matching, S&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;K-th Shortest Path, T&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;Dominator Tree, T&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;Global Minimum Cut, T&nbsp;</span>
  - <span style = "background-color:Silver;color:black">&nbsp;Chordal Graph, N&nbsp;</span>

### Mathematics
  - Number Theory
    - <span style = "background-color:Aqua;color:black">&nbsp;Sieve of Eratosthenes, D&nbsp;</span>
    - <span style = "background-color:Aqua;color:black">&nbsp;GCD / LCM, D&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Exponential by Squaring, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Modular Multiplicative Inverse, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Extended Euclidean Algorithm, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Fermat's Little Theorem / Euler's Theorem, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Extended Euler's Theorem, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Primitive Root, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Floor Sum, A&nbsp;</span>
    - Linear Congruences
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Extended Chinese Remainder Theorem, B&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Chinese Remainder Theorem, N&nbsp;</span>
    - Lucas's Theorem
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Basic, B&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Extended Lucas's Theorem, A&nbsp;</span>
    - Discrete Logarithm
      - <span style = "background-color:Orange;color:black">&nbsp;Baby-Step Giant-Step, A&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Pohlig-Hellman Alogrithm, N&nbsp;</span>
    - Arithmetic Function
      - <span style = "background-color:Lime;color:black">&nbsp;Linear Sieve, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Fraction Floor Decomposition, C&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Dirichlet Product, B&nbsp;</span>
      - <span style = "background-color:SkyBlue;color:black">&nbsp;Mobius Inversion Formula, B&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Du's Sieve, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Min25 Sieve, A&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Miller Rabin, T&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Pollard Rho, T&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Pell's Equation, N&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Unit Root Inversion, N&nbsp;</span>
    - Quadratic Residual
      - <span style = "background-color:Silver;color:black">&nbsp;Legendre Smbol, N&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Cipolla's Algorithm, N&nbsp;</span>
  - Linear Algebra
    - <span style = "background-color:Lime;color:black">&nbsp;Matrix Exponential by Squaring, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Gaussian Elimination, C&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Determinant, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Hamel Basis, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Inverse Matrix, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Linear Recurrence Relations with Constant Coefficients, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Matrix-Tree Theorem, A&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Berlekamp-Massey Algorithm, T&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;BEST Theorem, N&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Eigenvalue and Eigenvector, N&nbsp;</span>
  - Combinatorics
    - Binomial Coefficient
      - <span style = "background-color:Lime;color:black">&nbsp;Pascal's Triangle, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Multiset Binomial Coefficient, C&nbsp;</span>
    - <span style = "background-color:Lime;color:black">&nbsp;Binomial Theorem, C&nbsp;</span>
    - Probability Theory
      - <span style = "background-color:Lime;color:black">&nbsp;Additive Law of Expectation, C&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Conditional Probability, N&nbsp;</span>
    - Principle of Inclusion and Exclusion
      - <span style = "background-color:Lime;color:black">&nbsp;Basic, C&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Min-Max Principle of Inclusion and Exclusion, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Binomial Transform, A&nbsp;</span>
    - Common Sequence
      - <span style = "background-color:Aqua;color:black">&nbsp;Fibonacci Sequence, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Derangement, D&nbsp;</span>
      - <span style = "background-color:Aqua;color:black">&nbsp;Catalan Number, D&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Partition Number, C&nbsp;</span>
      - <span style = "background-color:Lime;color:black">&nbsp;Stirling Number, C&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Bell Number, N&nbsp;</span>
      - <span style = "background-color:Silver;color:black">&nbsp;Bernoulli Number, N&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Prufer Sequence, A&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Lindström–Gessel–Viennot Lemma, N&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Young Tableau, N&nbsp;</span>
  - Game Theory
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Sprague–Grundy Theorem, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Common Conclusion, B&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Partisan Game, N&nbsp;</span>
  - Polynomial
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Fast Fourier Transform / Number Theoretic Transform, B&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Lagrange Polynomial, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Generating Function, A&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Polynomial Templates, T&nbsp;</span>
    - Set Power Series
      - <span style = "background-color:Orange;color:black">&nbsp;Fast Walsh–Hadamard Transform, A&nbsp;</span>
  - Group Theory
    - <span style = "background-color:Lime;color:black">&nbsp;Permutation, C&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Burnside's Lemma, A&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;Pólya Enumeration Theorem, A&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;Linear Programming, T&nbsp;</span>

### String
  - <span style = "background-color:Lime;color:black">&nbsp;Knuth–Morris–Pratt Algorithm, C&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;String Hashing, C&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Trie, C&nbsp;</span>
  - <span style = "background-color:SkyBlue;color:black">&nbsp;Aho–Corasick Algorithm, B&nbsp;</span>
  - <span style = "background-color:Orange;color:black">&nbsp;Suffix Array, A&nbsp;</span>
  - <span style = "background-color:Orange;color:black">&nbsp;Suffix Automaton, A&nbsp;</span>
  - <span style = "background-color:Orange;color:black">&nbsp;Palindromic Tree, A&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;Extended KMP Algorithm, T&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;Manacher's Algorithm, T&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;Minimum Representation of String, T&nbsp;</span>
  - <span style = "background-color:Black;color:white">&nbsp;Lyndon Decomposition, T&nbsp;</span>
  - <span style = "background-color:Silver;color:black">&nbsp;Suffix Tree, N&nbsp;</span>

### Others
  - <span style = "background-color:Lime;color:black">&nbsp;Discretization, C&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Prefix Sum / Difference, C&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Reverse Pairs, C&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Line Sweep, C&nbsp;</span>
  - <span style = "background-color:Lime;color:black">&nbsp;Two Pointers, C&nbsp;</span>
  - <span style = "background-color:SkyBlue;color:black">&nbsp;Binary Lifting, B&nbsp;</span>
  - <span style = "background-color:SkyBlue;color:black">&nbsp;Ternary Search, B&nbsp;</span>
  - <span style = "background-color:SkyBlue;color:black">&nbsp;Subset / Superset Enumeration, B&nbsp;</span>
  - Search
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Pruning, B&nbsp;</span>
    - <span style = "background-color:SkyBlue;color:black">&nbsp;Half-Interval Search, B&nbsp;</span>
    - <span style = "background-color:Black;color:white">&nbsp;Dancing Links, T&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;A* Search, N&nbsp;</span>
  - Divide and Counquer
    - Divide and Conquer on Tree
      - <span style = "background-color:Orange;color:black">&nbsp;Centroid Decomposition, A&nbsp;</span>
      - <span style = "background-color:Orange;color:black">&nbsp;Center Edge Decomposition, A&nbsp;</span>
      - <span style = "background-color:Red;color:black">&nbsp;Dynamic Centroid Decomposition, S&nbsp;</span>
    - <span style = "background-color:Orange;color:black">&nbsp;CDQ's Divide and Counquer, A&nbsp;</span>
  - Randomized Algorithm
    - <span style = "background-color:HotPink;color:black">&nbsp;Hill Climbing, Y&nbsp;</span>
    - <span style = "background-color:HotPink;color:black">&nbsp;Simulated Annealing, Y&nbsp;</span>
    - <span style = "background-color:Silver;color:black">&nbsp;Genetic Algorithm, N&nbsp;</span>
  - <span style = "background-color:HotPink;color:black">&nbsp;Bruteforce Verification, Y&nbsp;</span>
  - <span style = "background-color:HotPink;color:black">&nbsp;Constant-Factor Optimization, Y&nbsp;</span>
  - <span style = "background-color:HotPink;color:black">&nbsp;I/O Optimization, Y&nbsp;</span>
  - <span style = "background-color:Silver;color:black">&nbsp;Matroid, N&nbsp;</span>
