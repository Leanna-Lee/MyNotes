# 网卡单双 NUMA 导致网络性能下降  
## 测试场景  
在两台 KVM 宿主机上各部署多台虚拟机，在虚拟机之间使用 iperf3 进行一对一跨宿主机打流测试。  
在测试中发现，3对 VM 在跨宿主机打流时每组网络带宽可达到940Mbps（接近限速1Gbps）；当 VM 数量增加到某一数值时，每组网络带宽突降，甚至低于100Mbps。　　 
## 问题原因
- 在没有 NUMA 影响的情况下，多对 VM 跨宿主机网络测试性能正常，每组带宽接近限速1Gbps；  
- 当有 NUMA 影响时，物理网口 bond 时使用了不同 NUMA Node 所对应的网卡。跨 NUMA Node 内存访问的高延迟导致发送时的锁同步竞争代价急剧增大，使得网口发送成为瓶颈，进而拉低每组 VM 之间的网络打流性能。
## 调试分析
系统性能出现问题，可能包括以下几个方面：
- CPU 长时间被占用运行，检查是否有 CPU 占用过高的瓶颈点；
- CPU 经常要等待运行，检查是否有很多任务处于待运行状态，进程调度代价过高；
- CPU 访存出现缓存未命中（Cache Miss）或者换页（Page Swap），导致高出一两个数量级的延迟；
- CPU 频繁 I/O 访问导致的睡眠等待时间过长；
- CPU 并发/并行处理时因为临界区同步频次过高导致的锁竞争。
```
使用top查看宿主机和虚拟机的CPU利用率；
使用perf查看调度延迟；
perf top -p <vhost_pid>
使用dmesg查看系统信息；
dmesg | grep "vhost-num (post" | grep -o 'CPU#[0-9]*' | sort | uniq -c
```
```
查看网卡 pci
lspci | grep -i eth
ethtool eth0
ls -l /sys/class/net

查看 bond 口对应的物理网卡是否跨 NUMA Node
numactl --hardware
cat /etc/sysconfig/network-scripts/ifcfg-bond0
cat /proc/net/bonding/bond0
cat /sys/class/net/eth0/device/numa_node
```
通过 perf 去实时监测 vhost 和 qemu-kvm 进程的 CPU 占用分布，发现多对虚拟机跨宿主机打流测试时，CPU 主要消耗在 _raw_spin_lock_irqsave（关中断并加自旋锁），高达60%-80%  

**来自开发小伙伴的分析：**    
- vhost 进程从虚拟机的 tap 口收到数据包之后，会通过 bond0 对应的active 物理网口 ethx 发送出去。_raw_spin_lock_irqsav 在加锁时会关中断并忙等待，占用过高说明锁竞争程度很激烈，导致 CPU 一直处于无效的忙等状态。在出现性能问题时，因为 _raw_spin_lock_irqsav 的影响，vhost 进程的 CPU 没有实际用于发包，导致发包性能急剧下降，同时关中断也会进一步影响网口收包，导致虚拟机之间通信性能进一步降低。_raw_spin_lock_irqsave 接口调用处于 net_dev_queue 到 net_dev_xmit 之间，并且是在 IOMMU 所对应的网卡 DMA 内存地址映射处理阶段，但 net_dev_queue 和 net_dev_xmit 这两个接口调用之间的延迟高达几十到几百微秒。
- 涉及到对不同 NUMA Node 上所对应的 iova 内存的处理。iova 查找和插入是在加锁后执行的，所以一旦出现跨 NUMA Node 的访问，就会有很大的延迟，进而占用更长时间的锁。因为多个虚拟机在大量发包，因此一个大的延迟导致的锁竞争会让多个 vhost 进程在发包时因为要加锁同步而处于忙等，进而导致大量包被延迟，这些大量延迟的包的发送在依次拿到锁后又大概率再次遇到跨 NUMA Node 的访问，从而导致更多包发送时的忙等。最终导致net_dev_queue 和 net_dev_xmit 这两个接口调用之间的高延迟。  
## 解决方法
**Secret**
　
