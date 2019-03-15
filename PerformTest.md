***[Performance Test]()***
- [性能指标]()
- [常用工具]()
  - [UnixBench]()
  - [coremark]()
  - [stream]()
  - [fio]()
  - [iPerf3]()
# Performance Test
## 性能指标
- CPU处理能力
- 内存带宽
- 磁盘IOPS、吞吐
- 网络带宽
## 常用工具
### UnixBench  
UnixBench is the original BYTE UNIX benchmark suite, updated and revised by many people over the years.  
  
The purpose of UnixBench is to provide a basic indicator of the performance of a Unix-like system; hence, multiple tests are used to test various aspects of the system's performance. These test results are then compared to the scores from a baseline system to produce an index value, which is generally easier to handle than the raw scores. The entire set of index values is then combined to make an overall index for the system.  
  
Some very simple graphics tests are included to measure the 2D and 3D graphics performance of the system.  
  
Multi-CPU systems are handled. If your system has multiple CPUs, the default behaviour is to run the selected tests twice -- once with one copy of each test program running at a time, and once with N copies, where N is the number of CPUs. This is designed to allow you to assess:  
- the performance of your system when running a single task
- the performance of your system when running multiple tasks
- the gain from your system's implementation of parallel processing
#### 1 下载地址
[https://github.com/qcsuper/byte-unixbench](https://github.com/qcsuper/byte-unixbench)  
[https://github.com/cloudharmony/unixbench](https://github.com/cloudharmony/unixbench)  
[https://s3.amazonaws.com/cloudbench/software/UnixBench5.1.3.tgz](https://s3.amazonaws.com/cloudbench/software/UnixBench5.1.3.tgz)
#### 2 安装和运行
`tar -xvzf UnixBench5.1.3.tgz`
`cd UnixBench`
`make all`
`./Run`
#### 3 参数和结果说明
---
### coremark
### STREAM
### FIO
### iPerf3
