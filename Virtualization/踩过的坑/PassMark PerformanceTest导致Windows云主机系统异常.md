# PassMark PerformanceTest导致 Windows 云主机系统异常
## 测试场景
建立 Windows 云主机，使用性能测试软件 PassMark PerformanceTest9.0 测试虚拟机的性能，在软件启动后，云主机系统出现关机、自动重启或蓝屏现象。
## 问题原因
PerformanceTest9.0 本身的bug，其需要获取 msr的值，此功能 qemu 并未提供。在宿主机上执行`dmesg | grep kvm`可看到如下报错信息：  
`kvm [4326]: vcpu0 unhandled rdmsr: 0xce`
## 解决方法
MSR
两种可选方法解决此问题：
1. 换成 PerformanceTest7.0；  
2. 在宿主机上修改此参数：  
`echo 1 > /sys/module/kvm/parameters/ignore_msrs`
