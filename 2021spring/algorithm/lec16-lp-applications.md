## Linear Programming: Application

对于以下的问题，通过 LP 形式表示：

- Shortest Path
- Max-flow Problem
- Set Cover
- Vertex Cover





## Shortest Path

最短路径算法最著名的是 Dijkstra 算法和 Bellman-Ford 算法。

**Formalization - 1**

来源于 Refs [1] ，基于 Bellman-Ford 算法的思想，将最短路径问题规约到 LP 问题。

给定 $G=(V,E)$ ，起点 $s$ 和终点 $t$ ，定义 $d_v$ 为 $s$ 到 $v$ 的最短距离，那么我们有：
$$
\begin{aligned}
& \max d_t \\
& d_v \le d_u + w(u,v), \text{ for each edge } (u, v) \in E \\
& d_s = 0 \\
& d_i \ge 0, \text{ for any } i \in V
\end{aligned}
$$


至于为什么目标函数是 $max$ ，我是真的看不懂了。



**Formalization - 2**

来源于 Refs [2] ，基于 Dijkstra 算法的规约。

> Let $\mathcal{S} = \{S \sube V: s \in S, t \notin S\}$ , that is $\mathcal{S}$ is the set of all $s-t$ cuts in the graph. Then we can model the shortest $s-t$ path problem with the following integer problem:
> $$
> \begin{aligned}
> \text{ min }  & \sum_{e \in E} w_ex_e \\
> \text{ s.t. } & \sum_{e \in \delta(S)} x_e \ge 1, & \forall{S \in \mathcal{S}} \\
> & x_e \in \{0, 1\}, & \forall{e \in E}
> \end{aligned}
> $$
> where $\delta(S)$ is the set of all edges that have one endpoint in $S$ and the other endpoint not in $S$.

其对偶形式：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210514155118.png" style="width:67%;" />

😅 看不懂，看不懂。



## Maximum flow

给定源点 $s$ 和汇点 $t$，对于图 $G = (V, E)$ 的一个流函数 $f$，其流量值为从 $s$ 出去的量减去流入 $s$ 的量（大部分情况是 0 ），即： $ val(f) = \sum_{v \in V} f_{sv} - \sum_{v \in V} f_{vs}$ 。

那么其线性规划形式为：
$$
\begin{aligned}
& \text{ maximize }  &\sum_{v \in V} f_{sv} - \sum_{v \in V} f_{vs} \\
& \text{ subject to } & f_{uv} \le c(u, v)  & \text{ for each } u, v \in V \\
& & \sum_{v \in V} f_{vu} = \sum_{v \in V} f_{uv}  & \text { for each } u \in V - \{s,t\} \\
& & f_{uv} \ge 0 & \text{ for each } u, v \in V
\end{aligned}
$$
该 LP 问题：

- 最多具有 $V^2$ 个变量（意味着 $G$ 为完全图，如果不是，那么增添容量为 0 的边，转换为等价的完全图）；
- 最多具有 $2V^2+V-2$ 个约束条件。
- 如果没有特别说明，流入 $s$ 的流量一般为 0 ，即 $\sum f_{vs} = 0$ .

一个例子：

|                           Example                            |                              LP                              |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210521125355.png" style="" /> | <img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210521125427.png"  /> |



注意到上述 LP 的第二个约束条件，我们必须排除 $s,t$ 这 2 个特殊点。

如果我们添加一个虚拟边 $t \rightarrow s$ ，那么可以有：
$$
\begin{aligned}
& \text{ maximize } & f_{ts} \\
& \text{ subject to } & f_{ij} \le c_{ij}, &\text{ for } (i,j) \in E \\
& & \sum_{(w,i) \in E}f_{wi} - \sum_{(i,z) \in E} f_{iz} \le 0, &\text{ for } i \in V \\
& & f_{ij} \ge 0, & \text{ for } (i,j) \in E
\end{aligned}
$$

## Min-Max Relations and Duality



本节介绍 LP 的对偶问题，及其二者之间的关系。

- Vertex Cover: 顶点没有权重，选取最少的顶点，覆盖所有边。
- Set Cover: 子集没有权重，选取最少的子集，覆盖所有的元素。

### VC

令 $x_v$ 表示顶点 $v$ 是否被选取。
$$
\begin{aligned}
\text{ min } & \sum_{v \in V} x_v \\
\text{ s.t. } & x_u + x_v \ge 1 & \forall{(u, v) \in E} \\
& x_v \in \{0, 1\} & \forall{v \in V}
\end{aligned}
$$
其 Dual LP 为：
$$
\begin{aligned}
\text{ max } & \sum_{e \in E} y_e \\
\text{ s.t. } & \sum_{e=(u, v) \in E} y_e \le 1 & \forall{v \in V} \\
& y_e \ge 0 & \forall{e \in E}
\end{aligned}
$$
其中 $y_e$ 表示边 $e$ 是否被选中，第一个约束表示每个顶点 $v$ 最多只能被其关联的边覆盖一次（这是匹配的定义）。

Dual LP 其实就是 Max-Matching 的 LP 形式，这就说明了 VC 和 Max-Matching 是对偶问题。

PS：如果是带权重的 VC 问题，那么把目标函数改为：
$$
\text{ min } \sum_{v \in V} w(v) \cdot x_v
$$
对应地，需要把 Dual LP 的约束改为 $\sum_{e=(u, v)\in E} y_e \le w(v)$ ，这时候表示的是 Max-Matching 每个顶点最多允许被匹配 $w(v)$ 次。



### SC

令 $x_S$ 表示子集 $S$ 是否选中。
$$
\begin{aligned}
\text{ min } & \sum_{S \in \mathcal{S}} x_S \\
\text{ s.t. } & \sum_{S:e \in S} x_S \ge 1 & \forall{e \in U} \\
& x_S \in \{0, 1\} & \forall{S \in \mathcal{S}}
\end{aligned}
$$
Dual LP:
$$
\begin{aligned}
\text{ max } & \sum_{e \in U} y_e \\
\text{ s.t. } & \sum_{e \in S} y_e \le 1 & \forall{S \in \mathcal{S}} \\
& y_e \ge 0 & \forall{e \in U}
\end{aligned}
$$
如果是带权重的 SC 问题：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210610162049.png" style="width:80%;" />

这里，可以把 $c(S)$ 理解为背包 $S$ 的容量（并且背包只允许放入 $S$ 所指定的物品），$y_e$ 理解为物品 $e$ 的体积，Dual LP 表示的是**使得装入背包的物品的体积之和最大**。有点像 Bin Packing 问题。



## More Examples

来源于 Refs [1] 的 Exercise 29.2 。

### Shortest Path Again

上面是指定起点和终点，此处只给出起点 $s$ ，求 $s$ 到所有顶点的最短路径，给出该问题的 LP 形式。
$$
\begin{aligned}
\text{ max } & \sum_{v \in V} d_v \\
\text{ s.t. } & d_v \le d_u + w(u, v) & \forall{(u, v) \in E} \\
& d_v \ge 0 & \forall{v \in V} \\
& d_s = 0
\end{aligned}
$$


### LP for Max-flow

给出下列 Max-flow 实例的 LP 表示。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210614191753.png" style="width:67%;" />

约束条件包括：

- 每个边的流量 $\le$ 容量
- 除了顶点 $s,t$ ，流入等于流出
- 任意顶点的流量 $\ge 0$ .

$$
\begin{aligned}
\text{ max } & f_{sv_1} + f_{sv_2} \\
\text { s.t. } & f_{sv_1} \le 16 \\
& f_{sv_2} \le 13 \\
& \dots \text{omit other edges} \\
& f_{sv_1} + f_{v_2v_1} = f_{v_1v_2} \\
& f_{sv_2} + f_{v_3v_2} = f_{v_2v_1} + f_{v_2v_4} \\
& \dots \text{omit vertices } v_3, v_4 \\
& f_e \ge 0, \forall{e \in E}
\end{aligned}
$$





### Bipartite Max-matching

给出二分图 $G = (L,R)$ 最大匹配的 LP 表示。（添加 $s,t$ 两个顶点，通过 Max-flow 的方法表示。）

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210614192836.png" style="width:50%"/>







## References

- [1] CLRS - Introduction to Algorithm (Section 29.2)
- [2] WS11 - The Design of Algorithm (Section 7.3)
- [3] VC Dual: http://pages.cs.wisc.edu/~shuchi/courses/787-F07/scribe-notes/lecture15.pdf
- [4] SC Dual: http://ac.informatik.uni-freiburg.de/lak_teaching/ws11_12/combopt/notes/set_cover.pdf