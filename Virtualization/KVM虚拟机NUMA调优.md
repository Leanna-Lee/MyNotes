# KVM 虚拟机 NUMA 调优
多 CPU 共同工作有三种架构：SMP、MPP、NUMA  
- SMP 技术
  SMP 即多个 CPU 通过一个总线访问存储器，因此 SMP 系统有时也被称为一致内存访问（UMA）结构体系。一致性意指无论在什么时候，处理器只能为内存的每个数据保持或共享唯一一个数值。在 SMP 中所有的处理器都是对等的，它们通过总线连接共享同一块物理内存。SMP 架构简单，但拓展性能差，在存储器接口达到饱和的时候，增加处理器并不能获得更高的性能，因此 SMP 方式支持的 CPU 个数有限
![SMP架构.png](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/image/SMP%E6%9E%B6%E6%9E%84.png)
- MPP 模式
- NUMA 技术  
  NUMA 模式则是每个处理器有自己的存储器，每个处理器也可以访问别的处理器的存储器。  
![NUMA架构.png](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/image/NUMA%E6%9E%B6%E6%9E%84.png)