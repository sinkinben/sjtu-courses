## Linear Programming based Approximation

主要内容：

- 基于 LP-rounding 设计 Set Cover 的 2 种近似算法
- 一种算法近似因子为 $f$ ，$f$ 是在给定子集中，出现次数最多的元素的次数
- 一种算法近似因子为 $\log{n}$ .



首先，我们给出 Set Cover (SC) 问题的定义：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210528210132.png" style="width:80%;" />



## f-approximation

包括 2 个方面的内容：

- Simple Rounding
- Randomized Rounding



### Simple Rounding

SC 问题的 LP 形式：
$$
\begin{aligned}
\text{ minimize }  & \sum_{S \in \mathcal{S}} c(S)x_{S} \\
\text{ subject to } & \sum_{S \ni e} x_S \ge 1, & {e \in U} \\
& x_S \in \{0, 1\}, & S \in \mathcal{S}
\end{aligned}
$$
$c(S)$ 表示子集的权值，$x_S$ 表示是否选择子集 $S$ 。

它的 LP Relaxation 形式为（直观的理解就是，原 SC 问题是 NPC 的，我们需要把条件放宽，不要求 $x_S$ 是一个整数，以便于我们使用近似算法解决）：
$$
\begin{aligned}
\text{ minimize }  & \sum_{S \in \mathcal{S}} c(S)x_{S} \\
\text{ subject to } & \sum_{S \ni e} x_S \ge 1, & e \in U \\
& x_S \ge 0, & S \in \mathcal{S}
\end{aligned}
$$
基于 LP Relaxation ，SC 问题可以这样解决：

![](https://gitee.com/sinkinben/pic-go/raw/master/img/20210604165734.png)

- Theorem - This algorithm achieves an approximation factor of $f$ for the set cover problem.

![](https://gitee.com/sinkinben/pic-go/raw/master/img/20210604170545.png)

我看不懂，但我又被震撼了一次。



### Randomized Rounding

讲随机算法，那是真的牛逼。

那我就来讲讲这个 Randomized Rounding 的 privilege 在哪里：

- 随机算法大概率可以保证比较高的性能，因为极端的 Coner Case 并不是特别常见的输入。
  - 比如快排的随机化优化，STL 里面 `nth_element` 算法的实现。
- 大部分随机算法，可以做到去随机化 (derandomized)，代价是损失一点平均性能。
- 随机算法可以使得我们更容易去逼近一个 NPC 问题的最优解。

对于 SC 问题，直觉上，基数大的子集是大概率出现在最优解中的（类似于贪心算法，子集的元素越多，我们就优先选取）。

TBD：我看不懂，但我又被震撼了一次。



## logn-approximation





## References

- [1] Vaz04 - Approximation Algorithms (Ch14 and Ch15)

