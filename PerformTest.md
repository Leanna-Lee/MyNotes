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
- Dhrystone：字符串处理
- Whetstone：浮点运算能力
- execl Throughput：execl函数系统调用
- File Copy
- Pipe Throughput
- Process Creation
- Shell Scripts
- System Call Overhead
  
（2）直接执行`./Run` 各测试项将运行两次
`Benchmark Run: 8 CPUs; 1 parallel process`  
`Benchmark Run: 8 CPUs; 8 parallel processes`  
（3）

### coremark
### Stream
### FIO
### iPerf3
