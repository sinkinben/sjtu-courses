## Simplex Algorithm

本文的主要内容是 Simplex Algorithm ，一个用于解决 Linear Programming 问题的算法。



## Introdcution

Simplex 算法的主要思路为：
$$
\begin{aligned}
& \text{let } v \text{ be any vertex of the feasible region} \\
& \text{while there is a neighbor } v′ \text{ of } v \text{ with better objective value:} \\
& \quad \quad \text{set } v = v'
\end{aligned}
$$
考虑一个具有 $n$ 个变量 $\bar{x} = (x_1, \dots, x_n)$  的**标准型**的线性规划问题，一个关键的问题是：我们如何定义 Neighbor Vertex ？

当 $n = 2$ 时，可行域是一个多边形，$n=3$ 时可行域是一个多面体。那么如果 $n \ge 4$ 时，在一个 $n$ 维的空间中，我们如何定义 Neighbor  Vertex ？

首先我们了解一些概念：

- 超平面 (Hyperplane): 在 n 维空间当中，一个 n-1 维的方程定义了一个超平面。比如在 3 维空间当中，$z = f(x,y)$ 可定义一个 2 维的超平面。
- 半空间 (Half-space): 对于一个特定的不等式约束（可以是 $<, \le, >, \ge$ 中的一种)），以 $f(x_1, \dots, x_n) \le b$ 为例，该不等式对应的超平面为 $x_n = f'(x_1, \dots, x_{n-1}) + b$ ，那么满足这个不等式约束的点，必然位于该超平面上或者超平面的某一侧，这些点所组成的空间叫 *Half-space* 。

上面 2 个概念可能代入 n=3 比较好理解，取自 Refs [1] 的一个例子。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210517204127.png" style="width:70%;" />

那么在 n 维的 Simplex 算法当中，顶点 (Vertex) 是若干个 Hyperplane 的交点（至少是 n 个）。

> - Each vertex is the unique point at which some subset of hyperplanes meet.
> - Pick a subset of the inequalities. If there is a unique point that satisfies them with equality, and this point happens to be feasible, then it is a vertex.
> - Each vertex is specified by a set of $n$ inequalities.

那么，一个顶点等价于 $n$ 个等式方程，如果 2 个顶点存在 $n-1$ 个等式方程是相同的，那么这 2 个顶点就是邻居 (Neighbor Vertices)。

> Two vertices are neighbors if they have $n-1$ defining inequalities in common.



## Simplex

### Standard form

对于一个标准型的 LP 问题：
$$
\begin{aligned}
& \text{ maximize } & \sum_{j=1}^{n} c_jx_j \\
& \text{ subject to } &  \\
& & \sum_{j=1}^{n}a_{ij}x_j \le b_i \text{,  for } i=1,2, \dots, m \\
& & x_j \ge 0 \text{,  for } i=1,2, \dots, m
\end{aligned}
$$
如果每个 $c_j$ 都有 $c_j < 0$ ，那么显然目标函数的最大值就是其**常数项**（这里是 0 ），当所有的 $x_j = 0$ 取到最大值。

Simplex 算法的主要思想是：

- 每一次迭代（后面会解释如何迭代），把当前的问题转换另外一个等价的 LP 问题，并使得目标函数的某个 $c_j$ 小于 0 。
- 当所有的 $c_j$ 都小于 0 ，算法结束。

在上一节的文章提到，Simplex 的过程类似于一个 hill-climbing 的过程，对于任意的标准型 LP 问题，选取 n 维空间的原点（即坐标全零的点）作为 hill-climbing 的起点。

假设当前 hill-climbing 的顶点是 $v = (x_1, \dots, x_n)$ ，对于每一次迭代，都包括下列操作：

- 选取一个 $c_j > 0$ 的 $c_jx_j$ 
- 逐步增加 $x_j$ 直到 $v$ 到达某个约束条件 $e$ 的边界
- $v$ 对应于 n 个方程，把当中的一个（下面的例子会解释选哪一个）替换为 $e$ ，那么可以得到邻居 $v'$
- 令 $v = v'$ 

大概思路是这么着，我们来看一下下 Refs [1] 的给出的一个例子。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210517211332.png" style="width:90%;" />

其 hill-climbing 的过程是这样的：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210517211525.png" style="width:40%" />

我们会发现，Simplex 的每一次的迭代都相当于转换了一次坐标系，把 $v$ 这个点「移动」到了新坐标系的原点。此外，这也是为什么起点需要取原点的原因，因为 $v$ 需要使得 LP 问题处于一个边界状态，而原点就是约束 $x_j \ge 0 (j \in [1,n])$ 这些条件的边界状态（所谓的 tight ）。

但这里仍然存在一个很严重的缺陷，如果某个不等式的 $b_i < 0$ ，那么显然我们的算法在「逐渐增大某个 $x_j$」这一步骤中会失败，因为坐标系的原点不再是 LP 问题的边界状态（形象一点地说，是越界了）。比如，我们将上述例子 Initial LP 中的 (3) 改为 $-x_1 + x_2 \le -3$ 。



### Slack form

上面是从 Standard Form 的角度来看的，下面我们从 Slack Form 的角度，来看一下 Simplex 算法又是如何工作的。

## Analysis







## References

- **[1] DPV - Algorithm (Section 7.6)**
- [2] CLRS - Introduction to Algorithm (Section 29.3)