## Lecture 01 - Introduction

Date: 2021/03/23

The main content of this class is to introduce the syllabus of this course.

And, few simple algorithms is introduced.

## Master Theorem

The *Master Theorem* is used for solving the time complexity of the following recursive algorithm:
$$
T(n) = aT(\frac{n}{b}) + f(n)
$$
In the above formula,

- $n$ represents the scale of the original problem
- $a(a\ge1)$ represents the number of the sub-problems
- $n/b(b\ge2)$ represents the scale of each sub-problem (assume that each sub-problem has the same scale)
- $f(n)$ is the the total time of decomposing the original problem into sub-problems and merging the solutions of each sub-problem.

Expecially, when $f(n) = O(n^d)$ for $d \ge 0$, we can get the following conclusion:
$$
T(n) = 
\begin{cases}
O(n^d)             &if \quad d>\log_{b}{a} \\
O(n^d\log{n})      &if \quad d=\log_{b}{a} \\
O(n^{\log_{b}{a}}) &if \quad d<\log_{b}{a} \\
\end{cases}
$$


## Merge Sort

|                          Recursion                           |                          Iteration                           |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210323161742.png"/> | <img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210323161834.png"/> |

**Time Complexity Analysis of recursion**

It's easy for us to see that:
$$
T(n) = 2T(\frac{n}{2}) + O(n)
$$
According to *Master Theorem*, the constant $d = 1 = \log_{b}{a}$, thus $T(n) = O(n\log{n})$ .

**Code for Recursion Version**

```cpp
// merge [l, m] and [m+1, r]
void merge(vector<int> &v, int l, int m, int r)
{
    vector<int> buf(r - l + 1, 0);
    int i = l, j = m + 1, k = 0;
    while (i <= m && j <= r)
    {
        if (v[i] < v[j]) buf[k++] = v[i++];
        else buf[k++] = v[j++];
    }
    while (i <= m) buf[k++] = v[i++];
    while (j <= r) buf[k++] = v[j++];
    copy(buf.begin(), buf.end(), v.begin() + l);
}
void mergesort(vector<int> &v, int l, int r)
{
    int n = v.size();
    if (l < 0 || r < 0 || r >= n || l >= n) return;
    if (l < r)
    {
        int m = l + (r - l) / 2;
        mergesort(v, l, m);
        mergesort(v, m + 1, r);
        merge(v, l, m, r);
    }
}
```

**Code for Iteration Version**

```cpp
void mergesort2(vector<int> &v)
{
    int n = v.size();
    for (int k = 1; k < n; k *= 2)
    {
        for (int l = 0; l < n - 1; l += 2 * k)
        {
            int m = min(l + k - 1, n - 1);
            int r = min(l + 2 * k - 1, n - 1);
            merge(v, l, m, r);
        }
    }
}
```



## Lower Bound of Sorting

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210323172010.png" style="width:80%;" />



## Shortest Path in Graph

**Problem**: Given $G = <V, E>$ and  $u, v \in V$, output the cost of shortest path from $u$ to $v$ in the graph $G$.



### BFS

BFS can work while all edges in $G$ having the same length. The time complexity is $O(|V|)$.

However, this situation usually don't exist.

Assume that every edge $e \in E$ has a length $l_e$. If $e = (u, v)$, it can also be denoted by symbol $l(u,v)$ or $l_{uv}$ .

There is a simple trick to handle this common situation. For any edge $e = (u, v)$ in edges set $E$, we add $l_e - 1$ dummy nodes between $u$ and $v$, then the length of each edge in $G$ is 1. This method will take time
$$
O(|V| + \sum_{e \in E}{l_e})
$$
**Example**

```
u --3--> v
// after adding dummy nodes
u --1--> d1 --1--> d2 --1--> v
```



### Dijkstra

It's classic algorithm in graph theory. 

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210324174127.png" style="width:67%;" />

**I think `decreasekey` is a mistake , it should be named `increasekey` .**

In the above pseudo code, `queue` actually means a heap, which is also call `priority_queue` in C++.

In this implementation, it will execute `deletemin()` for $|V|$ times, and execute `increasekey()` for $|V|+|E|$ times (at most).

And, there is a problem, which heap is the best?

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210324192940.png" style="width:80%;" />

This depends on whether the graph is sparse or dense.

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210324193351.png" style="width:80%;" />

**Code**

`unordered_map` is used to store the graph, the argument `V` represents the size of vertex set $V$, and `<start, end>` represents the start and the end of the shortest path.

And `priority_queue` is used for getting the minium cost here. The time complexity is $O(E\log{V})$ .

```cpp
struct node_t
{
    int vex, cost;
    node_t(int v, int c) : vex(v), cost(c) {}
    bool operator<(const node_t &n) const { return cost > n.cost; }
};
unordered_map<int, unordered_map<int, int>> graph;
vector<int> dijkstra(int V, int start)
{
    vector<bool> vis(V, false);
    vector<int> dis(V, 0x3f3f3f3f);
    dis[start] = 0;
    priority_queue<node_t> q;
    q.emplace(start, 0);
    while (!q.empty())
    {
        int u = q.top().vex;
        q.pop();
        if (vis[u]) continue;
        vis[u] = true;
        // graph[u][v] = c
        for (auto [v, c] : graph[u])
        {
            if (!vis[v] && dis[v] > dis[u] + c)
            {
                dis[v] = dis[u] + c;
                q.emplace(v, dis[v]);
            }
        }
    }
    return dis;
}
```

Exercises: [Leetcode - 743](https://leetcode-cn.com/problems/network-delay-time/description/?utm_source=LCUS&utm_medium=ip_redirect&utm_campaign=transfer2china) .

If there exists $e \in E$ and $l_e < 0$, then Dijkstra algorithm no longer works, we should use Bellman-Ford Algorithm at this situation. 

