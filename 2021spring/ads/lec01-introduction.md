## Introduction

老惯例了，每门课第一节总是 Introduction 。

## Taxonomy

Michael J. Flynn 对于 Parallel/Distributed System 的分类：

- SISD (Single Instruction Single Data)
  - Traditional uni-processor system
- SIMD (Single Instruction Multiple Data)
  - GPU, APU(AMD 早些年出的显卡产品 😅 ), Intel-SSE
- MISD (Multiple Instructions Single Data)
  - 😅 Generally not used and **doesn’t make sense**
- MIMD (Multiple Instructions Multiple Data)
  - Multi-core computers
  - <span style="color: #0366d6;">Parallel and Distributed systems</span>

😅 差不多得了，搁这排列组合。

对于 MIMD 可以进一步细分：

- Memory
  - Shared-memory system: multi-processes
  - Non-shared-memory system: multi-computers
- Inter-connect (二者的区别参考 Refs [2] )
  - 总线式 (Bus)
  - 交换式 (Switch)
- Delay/Bandwidth
  - Tightly coupled systems
  - Loosely coupled systems



## Cache Coherence

Cache Coherence ，也是所谓的「缓存一致性」，主要用于阐述 Multi-CPU 的场景下，Cache 与 Memory 之间的数据一致性问题（参考 Slide P25-30 给出的例子）。

- 现代多核处理器一般是 3 层 Cache 架构的，每个 CPU 都会有**独立的** L1-Cache 。
- CPU-CPU 之间的不一致：CPU-A 把 `x` 从内存读到 Cache，然后修改了 `x` （还没有写回内存），但 CPU-B 依然从内存读取 `x` 的旧值。
- CPU-Mem 之间的不一致：A 和 B 都把 `x` 读取到了各自的 Cache ，A 修改了 `x` 并完成写回内存，但 `B` 依然使用它的 Cache 中 `x` 的旧值。

为了避免上述的一些不一致问题，几乎所有的 Bus-based 架构的系统都会使用一个叫 “Snoopy Cache-Coherence Protocols” 的协议。



## Switched Multi-processors

Cache Coherence 是在 Bus-based 场景下产生的问题，但 Bus-based 的架构在 CPU > 8+ 的时候，处理 Cache-Coherence 变得十分复杂。

Switched Multi-processors 架构：

<img src="https://gitee.com/sinkinben/pic-go/raw/master/img/20210612203420.png" style="width:57%;" />

- 在这种架构下，访存方式使用 NUMA 而不是 Bus-based 的 UMA 。
- 通过 ccNUMA (Cache Coherent Non-Uniform Memory Access) 处理 Coherence 问题。
- 每个 CPU 都有独立的 Local Memory => Faster access local memory, slow access other CPU's memory.



## Multi-computers

个人理解，这里就是说如何把多台机器进行集群。

- Bus-based



- Switched



## Summary

一些值得研究的点：

- 一致内存访问 (Uniform Memory Access, UMA) 和非一致内存访问 (Non-uniform Memory Access)
- 







## References

- [1] https://ipads.se.sjtu.edu.cn/courses/ads/schedule.shtml (Lec. 1)
- [2] [Bus Based and Switched Micro-processor](https://www.idc-online.com/technical_references/pdfs/information_technology/Bus_Based_and_Switched_Microprocessor.pdf)







