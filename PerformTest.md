***[Performance Test](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#performance-test)***
- [性能指标](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#%E6%80%A7%E8%83%BD%E6%8C%87%E6%A0%87)
- [常用工具](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7)
  - [UnixBench](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#unixbench)
  - [coremark](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#coremark)
  - [Stream](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#stream)
  - [FIO](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#fio)
  - [iPerf3](https://github.com/Leanna-Lee/MyNotes/blob/master/PerformTest.md#iperf3)
# Performance Test
## 性能指标
- CPU处理：`UnixBench、coremark`
- 内存带宽：`Stream`
- 磁盘吞吐：`FIO`
- 网络带宽：`iPerf3`
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
 
**直接运行：**  
`./Run` 将运行UnixBench所有测试项  
  
**带参数运行：**
```
Run [-q | -v][-i <n>][-c <n>][test...]
-q Run in quiet mode.
-v Run in verbose mode.
-i Run n iterations for each test.
./Run -c -1 -c 4
Will Run a single-streamed pass, then a 4-streamed pass.
```
**执行指定测试项：**
```
./Run fstime -c 1
./Run pipe -c 2
./Run dhry2reg -c 8 
```
#### 3 参数和结果说明
（1）UnixBench 的运行结果、log 和输出保存在 results 目录，结果以 html 格式保存  

|测试项目|测试内容|
|:--|:--|
|Dhrystone|该测试侧重字符串处理，没有浮点运算。结果受硬件设计和软件优化影响大|
|Whetstone|测试浮点运算速度和效率，该测试包含多个典型的在科学计算中执行的操作的组合|
|Execl Throughput|这个测试测量每秒execl函数调用的次数。execl是exec函数家族的一部分,该函数用一个新的进程映像替换当前的进程映像|
|File Copy|测量使用不同大小的缓冲区，将数据从一个文件传输到另一个文件的速度|
|Pipe Throughput|管道是进程之间通信的最简单形式。管道吞吐量是指一个进程向管道写入512字节并读取回的次数|
|Pipe-based Context Switching|测试每秒两个进程通过一个管道交换一个不断增长的整数次数|
|Process Creation|进程创建实际上是指为新进程创建进程控制块和内存分配，因此这直接使用内存带宽|
|Shell Scripts|测试每秒进程可以并发获取一个 shell 脚本的 n 个副本的次数，n取值为1 2 4 8|
|System Call Overhead|测试进入和离开操作系统内核的开销，即执行系统调用的消耗|
|Graphical Tests|测试显卡2D和3D图形的大致性能|  

（2）直接执行`./Run` 各测试项将运行两次  
`Benchmark Run: 8 CPUs; 1 parallel process`  
`Benchmark Run: 8 CPUs; 8 parallel processes`  
  
（3）UnixBench得分计算说明  
- Index = Score / Baseline * 10  
- aver = Average(log<sub>e</sub>(Index)) `//各项Index的值以e为底取对数，再取平均值`  
- System Benchmarks Index Score = e<sup>aver^ `//e的aver次方`

|Test|Score|Unit|Time|Iters.|Baseline|Index|
|:--|:--|:--|:--|:--|:--|:--|
|Dhrystone 2 using register variables|213299834.2|lps|10.0 s|7|116700.0|18277.6|
|Double-Precision Whetstone|28813.5|MWIPS|10.0 s|7|55.0|5238.8|
|Execl Throughput|19268.2|lps|30.0 s|2|43.0|4481.0|
|File Copy 1024 bufsize 2000 mabxblocks||KBps|30.0 s|2|3960.0||
|File Copy 256 bufsize 500 maxblocks||KBps|30.0 s|2|1655.0||
|File Copy 4096 bufsize 8000 maxblocks||KBps|30.0 s|2|5800.0||
|Pipe Throughput||lps|10.0 s|7|12440.0||
|Pipe-based Context Switching||lps|10.0 s|7|4000.0||
|Process Creation||lps|30.0 s|2|126.0||
|Shell Scripts (1 concurrent)||lpm|60.0 s|2|42.4||
|Shell Scripts (8 concurrent)||lpm|60.1 s|2|6.0||
|System Call Overhead||lps|10.0 s|7|15000.0|||
|System Benchmarks Index Score:||||||4140.6|

### coremark
### Stream
### FIO
### iPerf3
