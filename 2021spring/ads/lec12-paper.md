## GraphChi

- Date: 2021/05/17
- Paper: [GraphChi: Large-Scale Graph Computation on Just a PC](https://ipads.se.sjtu.edu.cn/courses/ads/paper/ads-graphchi.pdf)



## Submission

**Synopsis**

This paper describes GraphChi, a disk-based graph processing system, which can process large-scale graph on a single machine. In GraphChi, the parallel sliding window algorithm is proposed to execute advanced MLDM algorithms on just one machine.



**Question**

> Please briefly describes how parallel sliding windows works.

**My Answer**

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210514204711.png" style="width:90%;" />

- Vertices are numbered from 1 to n, and divided into P intervals. 

- Each interval associates with a shard on disk. A "shard" stores all the edges that have destination in the interval, and it's sorted by source-id.

- For each interval $p$ :

  - Read $shard(p)$ into memory.
  - Since edges are sorted by source-vertex (the start-point of the edge), the out-edge are store in consecutive blocks in the other shards. Read the blocks in shards containing such out-edges into memory, which requires P-1 block-reads (at most).
  - For each vertex in interval $p$, execute the update-function and the modified blocks are written to disk (this operation is done in parallel).

  