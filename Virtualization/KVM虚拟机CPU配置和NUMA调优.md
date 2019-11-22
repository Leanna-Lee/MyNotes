***[KVM 虚拟机 CPU 配置 和 NUMA调优](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BACPU%E9%85%8D%E7%BD%AE%E5%92%8CNUMA%E8%B0%83%E4%BC%98.md#kvm-%E8%99%9A%E6%8B%9F%E6%9C%BA-cpu-%E9%85%8D%E7%BD%AE-%E5%92%8C-numa%E8%B0%83%E4%BC%98)***
- [1、宿主机的 NUMA 信息查看与配置](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BACPU%E9%85%8D%E7%BD%AE%E5%92%8CNUMA%E8%B0%83%E4%BC%98.md#1%E5%AE%BF%E4%B8%BB%E6%9C%BA%E7%9A%84-numa-%E4%BF%A1%E6%81%AF%E6%9F%A5%E7%9C%8B%E4%B8%8E%E9%85%8D%E7%BD%AE)  
- [2、虚拟机 NUMA 信息查看与配置](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BACPU%E9%85%8D%E7%BD%AE%E5%92%8CNUMA%E8%B0%83%E4%BC%98.md#2%E8%99%9A%E6%8B%9F%E6%9C%BA-numa-%E4%BF%A1%E6%81%AF%E6%9F%A5%E7%9C%8B%E4%B8%8E%E9%85%8D%E7%BD%AE)  
- [3、CPU 绑定操作方式](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BACPU%E9%85%8D%E7%BD%AE%E5%92%8CNUMA%E8%B0%83%E4%BC%98.md#3cpu-%E7%BB%91%E5%AE%9A%E6%93%8D%E4%BD%9C%E6%96%B9%E5%BC%8F)  
- [4、虚拟机 CPU 热添加](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BACPU%E9%85%8D%E7%BD%AE%E5%92%8CNUMA%E8%B0%83%E4%BC%98.md#4%E8%99%9A%E6%8B%9F%E6%9C%BA-cpu-%E7%83%AD%E6%B7%BB%E5%8A%A0)  
- [5、CPU host-passthrough]()
# KVM 虚拟机 CPU 配置 和 NUMA调优
多 CPU 共同工作有三种架构：SMP、MPP、NUMA  
- **SMP 技术**   
  SMP 即多个 CPU 通过一个总线访问存储器，因此 SMP 系统有时也被称为一致内存访问（UMA）结构体系。一致性意指无论在什么时候，处理器只能为内存的每个数据保持或共享唯一一个数值。在 SMP 中所有的处理器都是对等的，它们通过总线连接共享同一块物理内存。SMP 架构简单，但拓展性能差，在存储器接口达到饱和的时候，增加处理器并不能获得更高的性能，因此 SMP 方式支持的 CPU 个数有限。
![SMP架构.png](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/image/SMP%E6%9E%B6%E6%9E%84.png)
- **MPP 模式**   
  MPP 模式则是一种分布式存储器模式，能够将更多的处理器纳入一个系统的存储器。一个分布式存储器模式具有多个节点，每个节点都有自己的存储器，可以配置成为 SMP 模式，也可以配置成为非 SMP 模式。单个的节点相互连接起来就形成了一个总系统。MPP 可以近似理解成一个 SMP 的横向扩展集群。MPP 一般要依靠软件实现。
- **NUMA 技术（Non-Uniform Memory Access）**     
  NUMA 模式则是每个处理器有自己的存储器，每个处理器也可以访问别的处理器的存储器。如果说 SMP 相当于多个 CPU 连接一个内存池导致请求经常发生冲突的话，NUMA 就是将 CPU 的资源分开，以 Node 为单位进行切割，每个 Node 里有着独有的 core，memory 等资源，这也就导致了 CPU 在性能使用上的提升，但是同样存在问题就是 2 个 Node 之间的资源交互非常慢，当 CPU 增多的情况下，性能提升的幅度并不是很高。每个 Core 访问自己的存储器要比访问别的存储器快很多，速度相差 10~100 倍。
![NUMA架构.png](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/image/NUMA%E6%9E%B6%E6%9E%84.png)   
## 1、宿主机的 NUMA 信息查看与配置  
查看当前 CPU 硬件的情况：`numactl --hardware`  
查看每个节点的内存统计：`numastat`  
查看相关进程的 NUMA 内存使用情况：`numastat -c qemu-kvm`  

Linux 系统默认自动开启 NUMA 平衡策略。如果要关闭 Linux 系统的自动平衡，可以使用如下命令：  
`# echo 0  > /proc/sys/kernel/numa_balancing`    
开启自动 NUMA 平衡策略：  
`# echo 1 > /proc/sys/kernel/numa_balancing`  
## 2、虚拟机 NUMA 信息查看与配置  
查看或者修改虚拟机的 NUMA 配置：`virsh numatune`  
配置文件（以 32Core32G 虚拟机为例）：  
**此时用 stream 测虚拟机内存带宽约为 50-55 GB/s**    
```
<cputune>  
  ...   
</cputune>  
<numatune>  
  <memory node='strict' nodeset='0-1'/>  
  <memnode cellid='0' mode='strict' nodeset='0'/>
  <memnode cellid='1' mode='strict' nodeset='1'/>
</numatune>  

<cpu>  
  ...  
  <numa>  
    <cell id='0' cpus='0-15' memory='16777216' unit='KiB'/>  
    <cell id='1' cpus='16-31' memory='16777216' unit='KiB'/>   
  </numa>  
</cpu>
```  
## 3、CPU 绑定操作方式  
CPU 绑定可以解决物理机 CPU 利用率严重不平均的问题  
配置文件（以 32Core32G 虚拟机为例，物理机 2 Numa nodes，40 CPUs）：  
**此时用 stream 测虚拟机内存带宽提升到 59-65 GB/s**  
```
<vcpu placement='static'>32</vcpu>  
<cputune>   
  <vcpupin vcpu='0' cpuset='2'/>  
  <vcpupin vcpu='1' cpuset='22'/>  
  <vcpupin vcpu='2' cpuset='3'/>  
  <vcpupin vcpu='3' cpuset='23'/>  
  <vcpupin vcpu='4' cpuset='4'/>  
  <vcpupin vcpu='5' cpuset='24'/>  
  <vcpupin vcpu='6' cpuset='5'/>  
  <vcpupin vcpu='7' cpuset='25'/>  
  <vcpupin vcpu='8' cpuset='6'/>  
  <vcpupin vcpu='9' cpuset='26'/>  
  <vcpupin vcpu='10' cpuset='7'/>  
  <vcpupin vcpu='11' cpuset='27'/>  
  <vcpupin vcpu='12' cpuset='8'/>  
  <vcpupin vcpu='13' cpuset='28'/>  
  <vcpupin vcpu='14' cpuset='9'/>  
  <vcpupin vcpu='15' cpuset='29'/>  
  <vcpupin vcpu='16' cpuset='12'/>  
  <vcpupin vcpu='17' cpuset='32'/>  
  <vcpupin vcpu='18' cpuset='13'/>  
  <vcpupin vcpu='19' cpuset='33'/>  
  <vcpupin vcpu='20' cpuset='14'/>  
  <vcpupin vcpu='21' cpuset='34'/>  
  <vcpupin vcpu='22' cpuset='15'/>  
  <vcpupin vcpu='23' cpuset='35'/>  
  <vcpupin vcpu='24' cpuset='16'/>  
  <vcpupin vcpu='25' cpuset='36'/>  
  <vcpupin vcpu='26' cpuset='17'/>  
  <vcpupin vcpu='27' cpuset='37'/>  
  <vcpupin vcpu='28' cpuset='18'/>  
  <vcpupin vcpu='29' cpuset='38'/>  
  <vcpupin vcpu='30' cpuset='19'/>  
  <vcpupin vcpu='31' cpuset='39'/>  
</cputune>
```  
通过 CPU 绑定，可以避免虚拟机同一个 numa 的 vCPU 在物理机上跨 NUMA   
- 查看虚拟机 vCPU 和物理机 CPU 的对应关系：`virsh vcpuinfo`  
- 查看虚拟机可以使用哪些物理逻辑 CPU：`virsh emulatorpin`  
- 强制 vCPU 和物理机 CPU 一对一绑定：`virsh vcpupin <domain> <vcpu> <cpulist>`    
  
CPU 绑定实际上是 libvirt 通过 CGroup 来实现的，通过 CGroup 直接去绑定 KVM 虚拟机进程也可以。通过 CGroup 不仅可以做 CPU 绑定，还可以限制虚拟机磁盘、网络的资源限制。  
## 4、虚拟机 CPU 热添加
***前提：在创建虚拟机前，需要给虚拟机预留足够的 CPU***  
在虚拟机中查看 CPU：`cat /proc/interrupts`  
把虚拟机 CPU 数量在线修改成 5 个：`virsh setvcpus <domain> <cpus number> --live`  
在虚拟机中执行如下命令，将第 5 个 CPU 激活：  
`echo 1 > /sys/devices/system/cpu/cpu4/online`  
目前不支持 CPU 热去除，可以在虚拟机中关闭 CPU：  
`echo 0 > /sys/devices/system/cpu/cpu4/online`  
## 5、CPU host-passthrough  
在物理机 /usr/share/libvirt/cpu_map.xml 中可以查到 libvirt 支持的 CPU 型号、生产商信息和每种型号的 CPU 特定定义等信息。
