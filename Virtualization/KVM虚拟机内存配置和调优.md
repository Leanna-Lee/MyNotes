***[KVM 虚拟机内存配置和调优](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98.md#kvm-%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98)***  
- [1、宿主机内存合并（压缩）](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98.md#1%E5%AE%BF%E4%B8%BB%E6%9C%BA%E5%86%85%E5%AD%98%E5%90%88%E5%B9%B6%E5%8E%8B%E7%BC%A9)   
- [2、查看 KSM 运行情况](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98.md#2%E6%9F%A5%E7%9C%8B-ksm-%E8%BF%90%E8%A1%8C%E6%83%85%E5%86%B5)  
- [3、禁止个别虚拟机进行内存压缩](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98.md#3%E7%A6%81%E6%AD%A2%E4%B8%AA%E5%88%AB%E8%99%9A%E6%8B%9F%E6%9C%BA%E8%BF%9B%E8%A1%8C%E5%86%85%E5%AD%98%E5%8E%8B%E7%BC%A9)  
- [4、内存气球配置](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98.md#4%E5%86%85%E5%AD%98%E6%B0%94%E7%90%83%E9%85%8D%E7%BD%AE)
- [5、虚拟机内存大小限制](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98.md#5%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E5%A4%A7%E5%B0%8F%E9%99%90%E5%88%B6)  
- [6、内存巨页](https://github.com/Leanna-Lee/MyNotes/blob/master/Virtualization/KVM%E8%99%9A%E6%8B%9F%E6%9C%BA%E5%86%85%E5%AD%98%E9%85%8D%E7%BD%AE%E5%92%8C%E8%B0%83%E4%BC%98.md#6%E5%86%85%E5%AD%98%E5%B7%A8%E9%A1%B5)
# KVM 虚拟机内存配置和调优
宿主机的内存压缩主要采用 KSM（Kernel SamePage Merging）技术。  
- **COW**  
Copy on write：写时复制。  
一种计算机程序设计领域的优化策略。其核心思想是，如果有多个调用者（callers）同时请求相同资源（如内存或磁盘上的数据存储），他们会共同获取相同的指针指向相同的资源，直到某个调用者试图修改资源的内容时，系统才会真正复制一份专用副本（private copy）给该调用者，而其他调用者所见到的最初的资源仍然保持不变。这过程对其他的调用者都是透明的（transparently）。优点是如果调用者没有修改该资源，就不会有副本（private copy）被建立，因此多个调用者只是读取操作时可以共享同一份资源。  
- **KSM**  
Kernel SamePage Merging。KSM 让内核扫描正在运行中的所有程序，并比较它们的内存，如果发现它们的内存页有相同的，那么就把它们相同的内存页合并为一个内存页，并将其标识为“写时复制”，当标识为“写时复制”的内存页需要被修改时，内核就为其分配新的内存空间，并复制内存页到新的空间，在新的内存空间上进行修改。  
KSM 设计初衷就是为了给虚拟化节约内存的，因为如果虚拟机都使用相同的系统和运行类似的程序，那么每个虚拟机进程就会有很大一部分的内存页是相同的，这时候使用 KSM 技术就能大大降低内存的使用率，也方便 KVM 内存的过载使用。传说有人在16G内存的机器上，成功运行 52 个 1G 内存的 XP 虚拟机。  
**缺点：**  
（1）KSM 需要扫描相同的内存页，会消耗一定的计算机资源用于内存扫描，加重 CPU 的开销；  
（2）使用 KSM 还需要保证足够的内存交换空间，因为我们把多个内存页合并为一个，当内存耗尽，而恰好有内存页需要修改时，此时内存就溢出了，只能是频繁地使用 swap 交互，导致虚拟机性能下降严重。
## 1、宿主机内存合并（压缩）
KSM 在 CentOS6、CentOS7上是默认打开的，主要有两个服务：  
- KSM 服务  
- ksmtuned 服务
```
service ksm stop
service ksmtuned stop
chkconfig ksm off
chkconfig ksmtuned off
```  
## 2、查看 KSM 运行情况
You can verify KSM is in action, by checking for the existence of some of its /sys files, under /sys/kernel/mm/ksm/

They are:
```
pages_shared	how many shared pages are being used  
pages_sharing	how many more sites are sharing them i.e. how much saved  
pages_unshared	how many pages unique but repeatedly checked for merging  
pages_volatile	how many pages changing too fast to be placed in a tree  
full_scans	how many times all mergeable areas have been scanned  
```  
## 3、禁止个别虚拟机进行内存压缩
阻止宿主机将特定的虚拟机内存页合并，xml 配置文件如下：  
```
<memoryBacking>  
  <nosharepages/>
</memoryBacking>
```  
## 4、内存气球配置
KVM 的内存气球技术可以动态调节虚拟机内存大小，提高内存的利用率。  
虚拟机需要安装 virt ballon 驱动，内核开启 CONFIG_VIRTIO_BALLON。  
在虚拟机中查看：`lspci | grep "Virtio memory balloon`  
虚拟机 xml 配置文件如下：  
```
<memballoon model='virtio'>  
  <alias name='balloon0'/>
</memballoon>
```  
balloon 的膨胀与压缩：  
- 膨胀：把虚拟机的内存拿掉，还给宿主机。  
- 压缩：把宿主机的内存给虚拟机使用。  
```
查看虚拟机当前内存大小：  
# virsh qemu-monitor-command <domain> --hmp --cmd info balloon  
限定虚拟机内存大小为 2GB：  
# virsh qemu-monitor-command <domain> --hmp --cmd balloon 2048  
限定虚拟机内存大小为 4GB：  
# virsh qemu-monitor-command <domain> --hmp --cmd balloon 4096
```  
## 5、虚拟机内存大小限制
将虚拟机的内存限定在一定范围内  
```
# virsh memtune --help  
  NAME  
    memtune - Get or set memory parameters  
  
  SYNOPSIS
    memtune <domain> [--hard-limit <number>] [--soft-limit <number>] [--swap-hard-limit <number>] [--min-guarantee <number>] [--config] [--live] [--current]  

  DESCRIPTION
    Get or set the current memory parameters for a guest domain.  
    To get the memory parameters use following command:  
  
    virsh # memtune <domain>  

  OPTIONS  
    [--domain] <string>  domain name, id or uuid
    --hard-limit <number>  Max memory, as scaled integer (default KiB)  
    --soft-limit <number>  Memory during contention, as scaled integer (default KiB)  
    --swap-hard-limit <number>  Max memory plus swap, as scaled integer (default KiB)  
    --min-guarantee <number>  Min guaranteed memory, as scaled integer (default KiB)  
    --config         affect next boot  
    --live           affect running domain  
    --current        affect current domain  
```  
或者写到虚拟机 xml 配置文件中：  
```
<memory unit='KiB'>8388608</memory>  
<currentyMemory unit='KiB'>4194304</currentMemory>  
<memtune>  
  <hard_limit unit='KiB'>9437184</hard_limit>  
  <soft_limit unit='KiB'>7340032<soft_limit>  
  <min_guarantee unit='KiB'>4194304</min_guarantee>  
  <swap_hard_limit unit='KiB'>10488320</swap_hard_limit>  
</memtune>
```  
限制虚拟机对宿主机 swap 的使用。locked 阻止宿主机将 swap 内存分配给虚拟机。设置 locked 参数，必须在 memtune 中设置 hard_limit  
```
<memoryBacking>  
  <locked/>
</memoryBacking>
```
## 6、内存巨页
x86 默认内存页大小为 4KB。在虚拟内存管理中，内核维护一个将虚拟内存地址映射到物理地址的表，对于每个页面操作，内核都需要加载相关的映射。如果内存页很小，那么需要加载的页就会很多，导致内核会加载更多的映射表。使用“大内存页”，意味着所需要的页变少了，从而大大减少由内核加载的映射表的数量。通过启用“大内存页”，系统具只需要处理较少的页面映射表，从而减少访问/维护它们的开销。  
**x86 默认巨页大小是 2M**  
- 配置巨页数量：`sysctl vm.nr_hugepages=104000`  
- 查看巨页配置信息：  
`sysctl -a | grep -i huge`  
`cat /proc/meminfo | grep -i huge`  
- 查看巨页挂载信息：  
`mount | grep -i huge`  // hugetlbfs  
- 重启 libvirtd 服务：systemctl restart libvirtd  
- 关闭巨页配置：`sysctl vm.nr_hugepages=0`  
- 查看当前巨型页使用情况：`cat /proc/sys/vm/nr_hugepages`   
  
**虚拟机 xml 配置文件：**    
```
<currentMemory unit='KiB'>209715200</currentMemory>
<memoryBacking>

```  

如果要使用 1GB 的巨页，在 /etc/default/grub 和 /boot/efi/EFI/redhat/grub.cfg 配置文件中加入：  
`default_hugepagesz=1G hugepagesz=1G hugepages=200`  
  
让修改的 grub 配置生效：  
```
# grub2-mkconfig -o /etc/grub2.cfg  
# grub2-mkconfig -o /etc/grub2-efi.cfg
```