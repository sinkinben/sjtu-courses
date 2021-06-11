## Lecture 02 - Stable Matching

Date: 2021/03/24

这节课的主要内容是匹配问题，因为内容过于晦涩，所以就不用英文写了。

## 问题描述

给定集合 $M = \{m_1,...,m_n\}$ 和 $W = \{w_1, ..., w_n\}$, 并且 $|M| = |W| = n$ ，如何找到一个 *稳定匹配* $S$ ?

首先需要区分几个概念。

**匹配**：一个匹配 $S$ 是 $M$ 和 $W$ 的笛卡尔积 $M \times W$ 的子集，并且：

- 任意的 $m \in M$ ，在 $S$ 中最多出现一次。
- 任意的 $w \in W$ ，在  $S$ 中最多出现一次。

**最大匹配**：所有匹配当中，具有边数最大的匹配。

**完美匹配**：一个匹配 $S$ 当且仅当满足 $|S| = |M| = |W| = n$ ，那么匹配 $S$ 是完美匹配。



**不稳定对 (Unstable Pair)**

如果存在这样的喜好排序：

```
m  : w' > w
w' : m  > m'
```

不管 $m'$ 和 $w$ 的喜好排序如何，给定匹配集合是 $S = \{(m, w), (m', w')\}$ ，也就是说，我们「强行」让 $m, w'$ 都与次一级别的元素匹配（这两个是「相互喜欢」的），那么这时候说，$pair = (m, w')$ 相对于匹配集 $S$ 来说是不稳定的，$S$ 是一个非稳定匹配。

**一个不稳定对一定是「双方都把对方放在首位的」**。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210324211419.png" style="width:30%"/>

**稳定匹配**

设 $U = M \times W$，匹配集合 $S$ 是一个稳定匹配，当且仅当 $\forall p \in (U - S)$ , $p$ 相对于 $S$ 来说是稳定的。



## Gale-Shapley 算法

该算法用于求解一个**稳定匹配**，同时也是一个完美匹配。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210324212632.png" style="width:80%;" />

设 $|M| = |W| = n$ .

下面是一些 Obervation：

- O1: Man propose to woman in decreasing order of preference.
- O2: Once a woman is matched, the woman never becomes unmatched; only “trades up”.



- 证明：算法复杂度为 $O(n^2)$ .

每次循环，都选取出一个没有重复的 pair ，pair 数目最多为 $|M \times W| = n^2$ .



- 证明：Gale-Shapley 算法输出的是一个匹配。

根据**匹配**的定义证明。

> Man proposed only if unmatched.  =>  matched to $\le 1$ woman.
> Woman keeps only one best man.  => matched to $\le 1$ man.



- 证明：Gale-Shapley 算法输出的匹配中，所有的 Men 均得到匹配。

通过反证法证明。

假设算法终止时， $\exist{m_0 \in M}$ ，$m_0$ 枚举了其偏好列表的所有元素（相当于枚举了 $W$ 集合），但是仍然没有完成匹配。

因此 $\exist{w_0 \in W}$ 在算法终止时，$w_0$ 也没有完成匹配。根据 Observation-2 可得，$w_0$ 从没有被枚举过（因为一个 Woman 一旦匹配就不会回恢复单身），这与 $m_0$ 枚举了 $W$ 集合矛盾。



- 证明：Gale-Shapley 算法输出的匹配中，所有的 Woman 均得到匹配。

根据上一个证明，所有的 Men 都得到匹配，根据匹配的定义，可以得出所有的 Woman 都被匹配。



- 证明：Gale-Shapley 算法输出的匹配 $S$ 是一个稳定匹配。

设全集 $U = M \times W$ ，即证明 $\forall{p \in U-S}$ ，$p$ 相对于 $S$ 来说是一个稳定的 pair 。

通过反证法证明，一个 unstable-pair 一定是「把对方放在首位」的。

> Consider any pair $(m, w) \notin S$ :
>
> - Case-1: $m$ never proposed to $w$ .
>   - Then $m$ prefers $w'$ (compared with $w$ , $w' > w$).
>   - $(m, w)$ is not a unstable-pair.
> - Case-2: $m$ proposed to $w$ .
>   - $w$ rejected $m$ (either right away or later).
>   - $w$ prefers $m'$ (compared with $m$, $m' > m$) .
>   - $(m, w)$ is not a unstable-pair.
> - In either case, the pair $(m,w)$ is not a unstable-pair.



- 证明：算法输出的匹配对 Men 来说是最优的。

这个我看得半懂不懂了。