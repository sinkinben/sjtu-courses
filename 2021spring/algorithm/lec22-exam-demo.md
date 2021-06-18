## Exam Demo

考试样题，包括：

- DP (missing 2016)
- NP Problem (missing 2016)
- Network Flow
- LP
- Approximation Algorithm



## DP

### 2017 LCS

- (2017) 求 $x = x_1 \dots x_n$ 与 $y = y_1 \dots y_m$ 的最长公共子串。

`dp[i, j]` 表示以 $x_i$ 结尾和以 $y_j$ 结尾的最长公共子串的长度。

转移方程：
$$
dp[i,j] = \left\{
\begin{aligned}
& dp[i-1, j-1]+1, & \text{ if } x_i = y_j \\
& 0, & \text{ otherwise }
\end{aligned}
\right.
$$
初始条件：
$$
dp[i, 0] = 0 \\
dp[0, j] = 0
$$
问题所求即是 $\max{\{dp[i, j]\}}$ .



### 2018 Subset Sum

**1. DP(1)**

背包问题的特殊情形，当所有物品的价值等于它的体积，即是本题。

$dp[i, j]$ 表示在前 $i$ 个数字 $a_1 \dots a_i$ 中，子集和不超过 $j$ 的最大和。

转移方程：
$$
dp[i,j] = \left\{
\begin{aligned}
& dp[i-1, j], & \text{ if } j < a_i \\
& \max(dp[i-1, j], dp[i-1, j-a_i]+a_i), & \text{ if } j \ge a_i
\end{aligned}
\right.
$$
边界条件：
$$
dp[1, j] = (j \ge a_1) ? a_1 : 0 \\
$$

最后返回 `dp[n, t] == t` .



**2. DP (2)**

$dp[i,j]$ 表示在 $a_1, \dots, a_i$ 中，是否存在一个子集（允许为空集），其总和为 $j$ （用 true / false 表示）。

转移方程：
$$
dp[i, j] = \left\{
\begin{aligned}
& dp[i-1, j], & \text{ if } j < a_i \\
& dp[i-1, j] \text{ or } dp[i-1, j-a_i], & \text{ if } j \ge a_i
\end{aligned}
\right.
$$
初始条件：
$$
dp[0, j] = \text{ False} \\
dp[i, 0] = \text{ True }
$$
最后返回 $dp[n, t]$ .



**3. 回溯法**

```cpp
subsetSum(int a[], n, t)
{
    if (t == 0) return 1;
    if (n == 0) return 0;
    return subsetSum(a, n-1, t) || subsetSum(a, n-1, t-a[n-1]);
}
```





### 2019 Longest Palindromic

- (2019) 最长回文子串

$dp[i, j]$ 表示 $x[i, \dots, j]$ 是否回文（以 0/1 表示）。

转移方程：
$$
dp[i,j] =  dp[i+1, j-1] \land (x[i] == x[j]), \text{ where } j \ge i
$$
边界条件：
$$
dp[i][i] = 1
$$



## NP

### 2015 Clique

- 2015 - Clique Problem => Clique + Independent Set

在 Clique Problem 给定的图 G 加入 $k$ 个独立的点。



### 2017 Graph‐Isomorphism

- 2017 - 证明图同构是 NP 。

只需要给出一个多项式时间的验证算法。

图同构问题：

> 给定 $G_1$ 和 $G_2$ 是同构的，当且仅当：存在一个映射函数 $f: V_1 \rightarrow V_2$，满足：
> $$
> \forall{(u, v) \in E_1} \iff (f(u), f(v)) \in E_2
> $$

显然，给定一个这样的映射，形如这样的顶点对：
$$
a \in V_1 \rightarrow f(a) \in V_2
$$
扫描 $E_1$ 的每一条边 $e=(u, v)$ ，我们可以在 $O(n)$ 时间内检查所有的 $(f(u), f(v))$ 是否都在 $E_2$ 中。



### 2018 Hitting Set

- 2018 - 证明 Hitting Set 是 NPC 。

题意解析：给定全集 $U$ 和若干子集 $S_i$，求一个 $H$ 使得与所有 $S_i$ 的交集均不空，并要求 $|H|$ 最小。 

用 Vertex Cover 规约。

- 给定点覆盖的实例 $G = (V, E)$ ，我们需要找到一个点集 $H$ （大小不超过 $b$ ），它覆盖所有的边。
- 对于 $E$ 中的每一条边 $e_i = (u_i, v_i)$ ，构造一个集合 $S_i = \{u_i, v_i\}$ ，那么就有 $|E|$ 个这样的集合。
- 根据 VC 的定义，集合 $H$ 覆盖了所有的边，即 $S_i$ 至少有一个点在 $H$ 当中：

$$
H \cap S_i \ne \O \text{ for any } S_i
$$

- 因此，$H$ 也是 Hitting Set 的解。
- 上述规约过程可以在多项式时间内完成，VC 是 NPC 问题，因此 Hitting Set 也是 NPC 。



令 $x_e$ 表示元素 $e$ 是否选中， Hitting Set 的 LP 形式：
$$
\begin{aligned}
\text{ min } & \sum_{e \in U} x_e \\
\text{ s.t. } & \sum_{e \in S} x_e \ge 1 & \forall{S \in \mathcal{S}} \\
& x_e \in \{0, 1\} & \forall{e \in U}
\end{aligned}
$$
$x_e \in \{0, 1\}$ 可以 Relax 为 $x_e \ge 0$ .

Dual LP：
$$
\begin{aligned}
\text{ max } & \sum_{S \in \mathcal{S}} y_S \\
\text{ s.t. } & \sum_{e \in S} y_S \le 1 & \forall{e \in U} \\
& y_S \ge 0 & \forall{S \in \mathcal{S}}
\end{aligned}
$$


参考：https://courses.cs.ut.ee/MTAT.03.286/2017_spring/uploads/Main/Homework-6-spring2017-solv



### 2019 Efficient Recruiting Problem

- 2019 - 证明 Efficient Recruiting Problem 是 NPC 。

参考：

- [1] Reduce SC: https://courses.cs.washington.edu/courses/csep531/09wi/handouts/sol3.pdf
- [2] Reduce VC: http://web.pdx.edu/~arhodes/alg9.pdf

个人感觉，这道题有点表述不清，如果只是简单要求至少一个人会 $n$ 项运动，那么只需要把 $m$ 个申请扫描一遍即可（因为每份申请必然包括这个人会哪些运动的）。

个人理解的题意：

- 一共有 $N$ 个运动。
- 夏令营要求至少有一个人，他需要擅长 $n(n < N)$ 个运动。现收到 $m$ 份申请，每份包括了这个人会哪些运动。
- 给定一个整数 $k$ ，问：能否雇佣 $\le k$ 个人，把 $N$ 个运动全部覆盖，并且至少有一个人会 $n$ 项运动。

先证明 ER 问题是 NP 问题：

- 给定 $len$ 个元素的申请列表 list，每个元素是 $a_i = [s_1, s_2, \dots]$ ，表示第 $i$ 个人会哪些运动 $s_i$ .
- 判断 $len \le k$ ，若是，则继续下一步；否则 false 。
- 扫描每个 $a_i$ ，记录是否存在一个 $i$ 满足 $len(a_i) \ge n$ ，并判断 $\cup_{i=1}^{len} a_i = N$ .
- 显然上述过程可以在多项式时间完成。

从 VC 问题规约：

- VC 问题：给定 $G=(V, E)$ 和整数 $k$ ，求是否存在一个子集 $V' \sube V$ 满足 $V'$ 覆盖所有的边，并且 $|V'| \le k$ .
- 令 $E$ 中的任意一边 $e_i$ 表示一个运动 $s_i$ ，$V$ 中的任意一点 $v_i$ 表示一个 Counselor $c_i$ .
- 那么问题就转换为 ER ：是否存在一个大小 $\le k$ 的子集 $V'$ ，它覆盖了所有的运动，并存在至少一个元素 $v \in V'$，$deg(v) \ge n$ .
- 上述规约可在多项式时间完成。

从 SC 问题规约：

- SC 问题：给定一组元素 $U$ 和 $U$ 的若干子集 $S_i$ ，以及一个 $k$，选取 $\le k$ 个子集，覆盖 $U$ 。
- 令 $U$ 中的每一个元素 $u_i$ 对应一项运动 $s_i$ ，每个子集 $S_i$ 对应于每个候选人 $c_i$ ，这里 $S_i$ 表示的是 $c_i$ 擅长的运动。
- 那么问题转换为 ER: 选取 $\le k$ 个  $c_i$ ，要求 $\cup c_i = N$ ，并且存在至少一个 $c_i$，$|c_i| \ge n$ .



## Network Flow

- 2020 (For undergraduate) - 锦标赛

参考：

- [1] https://www.cs.princeton.edu/courses/archive/spr03/cs226/assignments/baseball.html
- [2] https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-046j-design-and-analysis-of-algorithms-spring-2015/lecture-notes/MIT6_046JS15_lec14B.pdf

构造一个二分图的最大流问题，即构造 $G = (s,t, L, R)$ .

- 令 $L$ 表示 $m$ 场比赛，$R$ 表示 $n$ 个球队。
- 对于每个比赛 $l = (i, j) \in L \text{ where } i,j \in R$ ，构造边 $(l, i)$ 和 $(l, j)$，容量均为 1 。
- 构造边 $s \rightarrow \forall{l \in L}$ ，$l = (i, j)$ ，该边容量是 1 。
- 构造边 $\forall{r \in R} \rightarrow t$ 。

TBD: 我不会了。





## LP

### 2015 Primal and Dual

- 2015 - 判断是否最优解并写出 Dual 。

这题很简单，但也有需要注意的点：

- 该解是最优解。
- Dual 中也需要有常数项 -3 。
- 根据 Strong Duality，Dual 的最优值与 Primal 的最优值相等，均为 0 。

### 2015 Facility Location Problem

参考：http://pages.cs.wisc.edu/~shuchi/courses/787-F09/scribe-notes/lec10.pdf

题意解析：

- 给定一个图 $G$，顶点代表城市，在这个图中选定一些城市作为 Facility ，要求这些 Facility 把所有的顶点都覆盖到。
- 建立一个 Facility 需要 $f_j$ 费用，连接 Facility 与某个城市的费用是 $c_{i, j}$ .
- 求最小费用，通过 LP 描述这个问题。

令：

- $x_i$ 表示 Facility 是否在城市 $i$ 建立。
- $y_{i, j}$ 表示城市 $j$ 是否被 Facility $i$ 覆盖。

那么 LP 为：
$$
\begin{aligned}
\text{ minimize } & \sum_{i} f_ix_i + \sum_{i, j} c_{i,j} \cdot y_{i,j} \\
\text{ subject to } & \sum_i y_{i,j} \ge 1 & \forall{j} \\
& x_i \ge y_{i,j} & \forall{i, j} \\
& x_i, y_{i,j} \in \{0, 1\} &\forall{i, j}
\end{aligned}
$$
其中：

- 第一个约束表示每个城市 $j$ 至少被一个 Facility $i$ 覆盖到。
- 第二个约束表示如果某个城市 $j$ 被 Facility $i$ 所覆盖 (i.e. $y_{i, j}=1$)，那么 $i$ 必然是一个 Facility (i.e. $x_i=1$ )。



### 2017 Set Multicover problem

参考：https://courses.cs.ut.ee/MTAT.03.286/2017_spring/uploads/Main/Homework-6-spring2017-solv

题意解析：

- 在 Set Cover 的基础上，要求每个元素 $e$ 被覆盖 $r_e$ 次（每个元素的 $r_e$ 是不一样的）。
- 每个子集 $S$ 可以使用无限次，每次耗费 $cost(S)$ .

令 $x_{S}$ 表示子集 $S$ 的使用次数，那么 Primal ILP 为：
$$
\begin{aligned}
\text{ min } & \sum_{S \in \mathcal{S}} c(S) \cdot x_{S} \\
\text {s.t.} & \sum_{S:e \in S} x_S \ge r_e & \forall{e \in U} \\
& x_S \in N & \forall{S \in \mathcal{S}}
\end{aligned}
$$
其 Relaxed LP 就是把 $x_S \in N$ 改为 $x_s \ge 0$ :
$$
\begin{aligned}
\text{ min } & \sum_{S \in \mathcal{S}} c(S) \cdot x_{S} \\
\text {s.t.} & \sum_{S:e \in S} x_S \ge r_e & \forall{e \in U} \\
& x_S \ge 0 & \forall{S \in \mathcal{S}}
\end{aligned}
$$
Dual LP:
$$
\begin{aligned}
\text{ max } & \sum_{e \in U} r_e \cdot y_e \\
\text{ s.t. } & \sum_{e \in S} y_e \le c(S) & \forall{S \in \mathcal{S}} \\
& y_e \ge 0 & \forall{e \in U}
\end{aligned}
$$

### 2018 Feedback Vertex Set Problem

参考：http://www-math.mit.edu/~goemans/PAPERS/GoemansWilliamson-1996-PrimalDualApproximationAlgorithmsForFeedbackProblemsInPlanarGraphs.pdf

（😅其实不用看试着自己写也能写出来的，多做几题，其实发现都差不多的）

题意解析：给定图 $G=(V,E)$ ，找到一个权重最小的子集 $V' \sube V$ ，使得去除 $V'$ 后的 $G$ 是一个森林（无环图）。

令：

- $x_v$ 表示顶点 $v$ 是否被选中。
- $\mathcal{C}$ 表示图 $G$ 所有环的集合。

Primal LP:
$$
\begin{aligned}
\text{ min }  & \sum_{i \in V} w_i \cdot x_i \\
\text{ s.t. } & \sum_{i \in C} x_i \ge 1 & \forall{C \in \mathcal{C}} \\
& x_i \in \{0, 1\} & \forall{i \in V}
\end{aligned}
$$
Relaxed LP:
$$
\begin{aligned}
\text{ min }  & \sum_{i \in V} w_i \cdot x_i \\
\text{ s.t. } & \sum_{i \in C} x_i \ge 1 & \forall{C \in \mathcal{C}} \\
& x_i \ge 0 & \forall{i \in V}
\end{aligned}
$$
Dual LP:
$$
\begin{aligned}
\text{ max } & \sum_{C \in \mathcal{C}} y_C \\
\text{ s.t. } & \sum_{i \in C} y_c \le w_i & \forall{i \in V} \\
& y_C \ge 0 & \forall{C \in \mathcal{C}}
\end{aligned}
$$


### 2018 Dual

- 首先，加入非负约束

$$
\begin{aligned}
\text{ max } & 6x_1 + 8x_2 + 5x_3 - 5x_3' + 9x_4 - 9x_4' + 5 \\
\text{ s.t. } & 2x_1 + x_2 + x_3 - x_3' + 3x_4 - 3x_4' > 5 \\
& x_1 + 3x_2 + x_3 - x_3' + 2x_4 - 2x_4' = 3 \\
& x_1, x_2, x_3, x_3', x_4, x_4' \ge 0
\end{aligned}
$$

- 然后写出各个矩阵，就能写出 Dual 。



### 2019 Equivalent LP

```
min  t
s.t. t = |x - y|
     y = x
```



### 2019 Steiner Forest Problem

这题就有点过分的了，看完 “参考” 也还不是很理解。

参考：

- [1] http://viswa.engin.umich.edu/wp-content/uploads/sites/169/2016/12/20.pdf
- [2] http://pages.cs.wisc.edu/~shuchi/courses/880-S07/scribe-notes/lecture14.pdf
- [3] http://www.ccs.neu.edu/home/rraj/Courses/7880/F09/Lectures/SteinerForest.pdf

定义：

- $\delta(S) = \{(u, v) \in E|u \in S, v \notin S\}$ 
- $\mathcal{S} = \{S \sube V | |S \cap \{s_i, t_i\}| = 1\}$

对于 $V$ 每一个子集 $S$ （一共 $2^V$ 个），我们选出那些与任意 $\{s_i, t_i\}$ 有且仅有 1 个元素相交的子集 $S$ ，放入 $\mathcal{S}$ 中。

令 $x_e$ 表示边 $e$ 是否选中：
$$
\begin{aligned}
\text{ min }  & \sum_{e \in E} c_e \cdot x_e \\
\text{ s.t. } & \sum_{e \in \delta(S)} x_e \ge 1 & \forall{S \in \mathcal{S}} \\
& x_e \in \{0, 1\} & \forall{e \in E}
\end{aligned} 
$$
第一个约束条件意味着任何 $e \in \delta(S)$ 都必然被选中，也就意味着 $(s_i, t_i)$ 必然相连。

Dual LP:
$$
\begin{aligned}
\text{ max } & \sum_{S \in \mathcal{S}} y_S \\
\text{ s.t. } & \sum_{e \in \delta(S)} y_S \le w_e & \forall{e \in E} \\
& y_S \in \{0, 1\} & \forall{S \in \mathcal{S}} 
\end{aligned}
$$
跟最短路径的 LP 表示方法类似。



## Approximation

近似算法。

### 2015 No triangles

顶点从 1 到 n 编号。

```text
Input: G = (V, E)
for i=1 to n:
	for j=i+1 to n:
		for k=j+1 to n:
			if (i, j, k) is a triangle:
				remove one of {(i, j), (j, k), (i, k)} randomly
				ans++
return ans
```

- 令 $m$ 表示图 $G$ 中，**没有公共边**的三角形的数目，显然 $m \le OPT$，因为 $OPT$ 还需要去除有公共边的三角形。
- 上述算法中，去除每一条边，去除的三角形不超过 2 个，因此 $ans \le 3m$ .
- 显然，上述输出的解 $ans \le 3m \le 3 OPT$  .



### 2015 Minimum Makespan Scheduling

令 `q` 是一个代表机器的优先队列，总是把 **工作时间最短** 的机器放在队首。

```
Input: p[1,...,n] and integer m
sort(p[1,...,n]) in descending order
// 每个机器初始的工作时间都是 0 
for i=1 to m:
	q.push(0)
// 给每个 job 分配机器
for i=1 to n:
	m = q.pop()
	m += p[i]
	q.push(m)
return max(q)
```

设上述算法输出的解为 $A$，那么：
$$
A \le \frac{\sum_{i=1}^{n}p_i}{m} + \max(p[1, \dots, n]) \le OPT + OPT = 2OPT
$$




### 2017 Maximum Cut

作业题，参考 Ex5 .



### 2017 Metric TSP

作业题，参考 Ex5 .

PS: 因为 TSP 也是一个 2-matching ，而 $M$ 已经是最小的 2-matching 了，因此 $OPT_{tsp} \ge M$ .



### 2017 Cardinality Vertex Cover

令 $T$ 是 DFS-Tree，$L$ 是 $T$ 的所有叶子结点的集合，$n$ 是顶点总数。

显然 $V-L$ 覆盖了 $G$ 的所有边。下面证明该算法的近似因子为 2 ：

- 该算法输出的解为 $n - L$，我们需要证明 $n-L \le 2OPT$ .
- 在最优解 $OPT$ 中，显然不包括那些叶子结点。因为叶子结点只能覆盖一边，而叶子的父亲可以同时覆盖 $\ge 2$ 边，这种情况下，都是耗费一个顶点，显然选择父亲结点更优。
- 对于剩下的顶点 $V-L$ 中，每个顶点至少关联 2 个边，要覆盖所有的边，至少需要 $n - L/2$ 个顶点，因此 $OPT \ge n-L/2$ .



### 2018 Knapsack

作业题，参考 Ex5.



### 2019 SC-Tight

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210602193438.png" style="width:70%;" />



### 2019 Hitting Set

参考：https://stanford.edu/~rezab/discrete/Notes/ws4.pdf

令 $x_e$ 表示元素 $e \in A$ 是否被选中。

Primal LP:
$$
\begin{aligned}
\text{ min } & \sum_{e \in A} x_e w_e \\ 
\text{ s.t. } & \sum_{e \in B_j} x_e \ge 1 & j=1,\dots, m \\
& x_e \in \{0, 1\} & \forall{e \in A}
\end{aligned}
$$
Relaxed LP:
$$
\begin{aligned}
\text{ min } & \sum_{e \in A} x_e w_e \\ 
\text{ s.t. } & \sum_{e \in B_j} x_e \ge 1 & j=1,\dots, m \\
& 0 \le x_e \le 1 & \forall{e \in A}
\end{aligned}
$$
令 $S = \{a_i | x_i \ge 1/b\}$ ，证明 $S$ 是一个有效的 Hitting Set 且 $w(S) \le b \cdot OPT$ 。

