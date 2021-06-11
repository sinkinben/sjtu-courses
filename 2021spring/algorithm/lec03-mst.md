## Lecture 03 - Minimum Spanning Tree

这节课逃课去聚餐了 #(逃 .

主要内容是最小生成树。

## Concepts

**Cut and cutset**

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210404203208.png" style="width:70%;" />

**Quiz** Let C be a cycle and let D be a cutset. How many edges do C and D have in common? Choose the best answer. (An even number)

**Tips**: 只需要考虑 Cut 中的点，在 Cycle 中是否相邻，即可证明。

从「图形」的直观上考虑，给定一个 Cut S，那么按照定义， S 中任意 2 点形成的边必不属于 Cutset 。Cutset 中的边，一端在 S 内，另外一端在 S 外（即在 V - S 中）。考虑 S 中的某一个点，从 S 出发，在环上走一圈，每「出去」一次必然还是需要「回来」一次才能成环，因此交集总是偶数的。

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210404204858.png" style="width:50%;" />

**Spanning Tree Definition**

Let $H = (V,T)$ be a subgraph of an undirected graph $G = (V,E)$. $H$ is a spanning tree of $G$ if $H$ is both acyclic and connected.

Properties:

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210404205501.png" style="width:67%;" />

**Cayley's Theorem**

完全图的生成树的数目为 $n^{n-2}$ .



**Fundamental Cycle**

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210404210653.png" style="width:67%;" />

**Fundamental Cutset**

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210404210722.png" style="width:67%;" />

Also have: If $c_e < c_f$, then $(V,T)$ is not a MST.



## Algorithm

例题：[1584. 连接所有点的最小费用](https://leetcode-cn.com/problems/min-cost-to-connect-all-points/) 。

### Prim

Prim 伪代码描述：

```cpp
Prim(G, start):
	vis = {start}
	notvis = V - {vis}
	mst = 0
	while (|vis| != |V|):
		minnode = 与 vis 相连的, 位于 notvis 的最小权值的点
		mst += cost
		remove minnode from notvis
		insert minnode into vis
	return cost
```

使用堆优化的 Prim 算法，邻接链表作为图的数据结构。

最小堆使得可在 $O(1)$ 时间找到 `vis` 和 `notvis` 两集合间的「最短边」，也可以用 `set` 实现。

使用堆优化的 Prim 每次向堆中插入一个点（最多插入 $V$ 个点），需要 $O(\log{V})$，并且需要遍历所有的边，因此复杂度是 $O(E\log{V})$ .

显然，这种优化适用于稀疏图，因为如果是稠密图 `for (auto [x, c]: g[vex])` 这一行每次都会遍历所有的点，相当于 $O(V^2\log{V})$ 的复杂度。因此有：

- 稠密图：用朴素 Prim （没有优化的）
- 稀疏图：用 Kruskal 或者堆优化的 Prim

显然 Leetcode 上的这题，其实不优化 Prim 是更好的。

```cpp
typedef pair<int,int> node_t;
struct cmp 
{
    bool operator()(const node_t &a, const node_t &b) const 
    { return a.second > b.second; }
};
class Solution {
public:
    const int inf = 0;
    int minCostConnectPoints(vector<vector<int>>& points) 
    {
        auto dis = [](vector<int> &v1, vector<int> &v2){
            return abs(v1[0]-v2[0]) + abs(v1[1]-v2[1]);
        };
        int n = points.size();
        vector<vector<node_t>> g(n);
        for (int i=0; i<n; i++)
        {
            for (int j=i+1; j<n; j++)
            {
                int weight = dis(points[i], points[j]);
                g[i].emplace_back(j, weight), g[j].emplace_back(i, weight);
            }
        }
        return prim(g, 0);
    }

    int prim(vector<vector<node_t>> g, int start)
    {
        int n = g.size();
        vector<int> vis(n, false);
        int cnt = 0, mst = 0;
        priority_queue<node_t, vector<node_t>, cmp> q;
        q.emplace(start, 0);
        while (!q.empty() && cnt < n)
        {
            auto [vex, cost] = q.top();
            q.pop();
            if (vis[vex]) continue;
            vis[vex] = true;
            mst += cost;
            cnt++;
            for (auto [x, c]: g[vex])
                if (!vis[x]) q.emplace(x, c); 
            // 此处 [x, c] 表示的是顶点 x 离 vis 集合的最短距离，为什么没有取 min 的操作？
            // 因为优先队列总是会选取 cost 最小的出队，所以队列中有冗余的顶点 x 也没关系
            // 循环会因为 cnt < n 达到边界而结束
        }
        return mst;
    }
};
```



### Kruskal

使用并查集和堆优化的 Kruskal 。

主要思想：把边从小到大排序，每次取一边 `<x, y>` ，如果 `x, y` 已连通则不添加，否则 `<x, y>` 必然是 MST 的一边。

复杂度主要是在排序操作上，因此为 $O(E\log{E})$ ，所以比较适合稀疏图。

```cpp
struct node_t
{
    int x,y,cost;
    node_t(int xx, int yy, int cc):x(xx), y(yy), cost(cc){}
    bool operator<(const node_t &node) const { return cost > node.cost; }
};
class Solution {
public:
    vector<int> root;
    int find(int x) { return root[x] == -1 ? x : root[x] = find(root[x]); }
    int minCostConnectPoints(vector<vector<int>>& points) {
        auto dis = [](vector<int> &a, vector<int> &b){ return abs(a[0]-b[0])+abs(a[1]-b[1]); };
        int n = points.size();
        root.resize(n, -1);
        priority_queue<node_t> q;
        for (int i=0; i<n; i++)
            for (int j=i+1; j<n; j++)
                q.emplace(i, j, dis(points[i], points[j]));
        return kruskal(q);
    }
    int kruskal(priority_queue<node_t> &q)
    {
        int mst = 0;
        while (!q.empty())
        {
            auto [x,y,cost] = q.top();
            q.pop();
            x = find(x), y = find(y);
            if (x == y) continue;
            mst += cost;
            root[y] = x;
        }
        return mst;
    }
};
```

