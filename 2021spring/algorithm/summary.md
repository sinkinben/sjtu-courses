## Summary

**常见算法的复杂度**

- GS 稳定匹配算法：$O(n^2)$
- 强连通分量 Tarjan 算法: $O(V+E)$ .
- MST
  - Prim: 朴素的 Prim 是 $O(V^2)$ ，堆优化后为 $O(E\log{V})$ .
  - Kruskal: $O(E \log{E})$ .
- 最短路
  - 单源最短路 Dijkstra: $O(V^2)$，堆优化后为 $O(E \log{V})$ ，Dijkstra 与 Prim 是类似的。
  - 多源最短路 Floyd: $O(V^3)$ .
- 最大流
  - Ford-Fulkerson: $O(VEC)$ .



**NP and Reduction**

- 把 A 规约到 B ，证明 B 是 NPC 的。
  - 其实就是把 A 和 B 的输入输出对应起来。
  - 从 A 出发，在 A 的输入上构造，变成 B 的输入。
  - 说明经过变换后的 A ，它的解可以与 B 对应起来。
  - 要求在多项式时间内完成。
- 如何证明一个问题 $L$ 是 NP 问题？
  - 证明 L 属于 NP 问题：给出一个多项式时间的验证算法（注意不需要证明 L 无法在多项式内解决，这并不是 NP 问题的要求）。
  - 证明 L 属于 P 问题：给出一个多项式的求解算法。
  - 证明 L 是 NPC 问题：把一个已证明是 NPC 的问题 $L'$ 规约到 $L$。
- 常见的 NPC 问题：VC、SC、IS、3SAT、Longest Path、TSP

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210416191049.png" style="width:67%;" />

**近似算法**

- Tight Example 需要对于所有的 $n$ 都成立，不能是一个具体的 example 。
- 证明近似因子为 $k$：
  - 最小化问题，证明 $OPT \le \mathcal{A} \le k OPT$ .
  - 最大化问题，证明 $OPT \ge \mathcal{A} \ge \frac{1}{k} OPT$ .
  - 随机算法， $\textbf{E}[\mathcal{A}]$ 替代 $\mathcal{A}$ 去证明。



**LP 问题**

- 写 Dual 时，如果 Primal LP 的目标函数有常数项，那么在 Dual 同样保留这个常数项（并且符号也不需要改变）。
  - Dual 记得要：`max -> min`，并且 $\le \quad \rightarrow \quad \ge$ .





TODO:

- 流问题的应用（如何从其他问题转换到流问题）
- 规约（lec08）
- Simplex
- 用 LP 表示图论问题
