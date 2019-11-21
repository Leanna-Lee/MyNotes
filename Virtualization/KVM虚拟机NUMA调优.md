# KVM 虚拟机 NUMA 调优
多 CPU 共同工作有三种架构：SMP、MPP、NUMA  
- **SMP 技术** 
  SMP 即多个 CPU 通过一个总线访问存储器，因此 SMP 系统有时也被称为一致内存访问（UMA）结构体系。一致性意指无论在什么时候，处理器只能为内存的每个数据保持或共享唯一一个数值。在 SMP 中所有的处理器都是对等的，它们通过总线连接共享同一块物理内存。SMP 架构简单，但拓展性能差，在存储器接口达到饱和的时候，增加处理器并不能获得更高的性能，因此 SMP 方式支持的 CPU 个数有限。
![SMP架构.png](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/image/SMP%E6%9E%B6%E6%9E%84.png)
- **MPP 模式  
  MPP 模式则是一种分布式存储器模式，能够将更多的处理器纳入一个系统的存储器。一个分布式存储器模式具有多个节点，每个节点都有自己的存储器，可以配置成为 SMP 模式，也可以配置成为非 SMP 模式。单个的节点相互连接起来就形成了一个总系统。MPP 可以近似理解成一个 SMP 的横向扩展集群。MPP 一般要依靠软件实现。
- NUMA 技术（Non-Uniform Memory Access）    
  NUMA 模式则是每个处理器有自己的存储器，每个处理器也可以访问别的处理器的存储器。如果说 SMP 相当于多个 CPU 连接一个内存池导致请求经常发生冲突的话，NUMA 就是将 CPU 的资源分开，以 Node 为单位进行切割，每个 Node 里有着独有的 core，memory 等资源，这也就导致了 CPU 在性能使用上的提升，但是同样存在问题就是 2 个 Node 之间的资源交互非常慢，当 CPU 增多的情况下，性能提升的幅度并不是很高。每个 Core 访问自己的存储器要比访问别的存储器快很多，速度相差 10~100 倍。
![NUMA架构.png](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/image/NUMA%E6%9E%B6%E6%9E%84.png)