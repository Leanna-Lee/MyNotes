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
- Windows操作系统：WinSAT
  Windows System Assessment Tools
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
**后台运行：**
`nohup ./Run &` 
  
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
- System Benchmarks Index Score = e<sup>aver</sup> `//e的aver次方`

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

#### 4 一些注意事项
UnixBench代码默认所能测试的系统最大CPU数有限   
可修改Run脚本中的maxCopies  
```
my $testCats = {
    'system'    => { 'name' => "System Benchmarks", 'maxCopies' => 100 },
    '2d'        => { 'name' => "2D Graphics Benchmarks", 'maxCopies' => 1 },
    '3d'        => { 'name' => "3D Graphics Benchmarks", 'maxCopies' => 1 },
    'misc'      => { 'name' => "Non-Index Benchmarks", 'maxCopies' => 100 },
};
```
------
### coremark  
---
### Stream  
The STREAM benchmark is a simple synthetic benchmark program that measures sustainable memory bandwidth (in MB/s) and the corresponding computation rate for simple vector kernels.  
  
The table below shows how many Bytes and FLOPs are counted in each iteration of the STREAM loops.  
  
The test consists of multiple repetitions of four the kernels, and the best results of (typically) 10 trials are chosen.  
  
    ------------------------------------------------------------------  
    name        kernel                  bytes/iter      FLOPS/iter  
    ------------------------------------------------------------------  
    COPY:       a(i) = b(i)                 16              0  
    SCALE:      a(i) = q*b(i)               16              1  
    SUM:        a(i) = b(i) + c(i)          24              1  
    TRIAD:      a(i) = b(i) + q*c(i)        24              2  
    ------------------------------------------------------------------  

#### 1 下载地址  
[http://www.cs.virginia.edu/stream/ref.html#start](http://www.cs.virginia.edu/stream/ref.html#start)  
[http://www.nersc.gov/users/computational-systems/cori/nersc-8-procurement/trinity-nersc-8-rfp/nersc-8-trinity-benchmarks/stream/#toc-anchor-2](http://www.nersc.gov/users/computational-systems/cori/nersc-8-procurement/trinity-nersc-8-rfp/nersc-8-trinity-benchmarks/stream/#toc-anchor-2)  
#### 2 安装和运行  
`yum -i install gcc	//安装gcc，如果没有`   
`tar -xvf stream.tar`  
`vim stream.c	//如下图所示`   
`vim Makefile	//如下图所示`  
`编辑 make`  
`执行 ./stream_c.exe`  

修改测试所用Array大小N，80000,000大概使用1.8G内存空间  
修改测试次数NTIMES  
`vim stream.c`
```C  
#ifndef N
#    define N    80000000
#endif
#ifndef NTIMES
#    define NTIMES    500
#endif
#ifndef OFFSET
#    define OFFSET    0
#endif
```  
`vim Makefile`
```C  
CC = gcc
#FF = ftn

CFLAGS = -O2
FFLAGS = -O2

#PGI
#CFLAGS += -mp
#FFLAGS += -mp

#GNU
CFLAGS += -fopenmp
#FFLAGS += -fopenmp

#INTEL
#CFLAGS += -openmp
#FFLAGS += -openmp

#all: stream_f.exe stream_c.exe

#stream_f.exe: stream.f mysecond.o
#	$(CC) $(CFLAGS) -c mysecond.c
#	$(FF) $(FFLAGS) -c stream.f
#	$(FF) $(FFLAGS) stream.o mysecond.o -o stream_f.exe

stream_c.exe: stream.c
	$(CC) $(CFLAGS) stream.c -o stream_c.exe

clean:
	rm -f stream_f.exe stream_c.exe *.o
```

------
### FIO  
FIO is an I/O tool meant to be used both for benchmark and stress/hardware verification.   

It has support for different types of I/O engines (sync, mmap, libaio, posixaio, SG v3, splice, null, network, syslet, guasi, solarisaio, and more), I/O priorities (for newer Linux kernels), rate I/O, forked or threaded jobs, and much more.  

It can work on block devices as well as files. fio accepts job descriptions in a simple-to-understand text format. Several example job files are included. fio displays all sorts of I/O performance information, including complete IO latencies and percentiles.   

Fio is in wide use in many places, for both benchmarking, QA, and verification purposes. It supports Linux, FreeBSD, NetBSD, OpenBSD, OS X, OpenSolaris, AIX, HP-UX, Android, and Windows.
#### 1 下载地址  
[http://freshmeat.sourceforge.net/projects/fio](http://freshmeat.sourceforge.net/projects/fio)  
[https://github.com/axboe/fio/](https://github.com/axboe/fio/)  
#### 
#### 2 安装和运行
（1）centOS yum 源直接安装 `yum install -y fio`  
（2）rpm 包安装 `rpm -ivh fio-xxx-xxx.x86_64.rpm`  
- 运行  
```
fio -name=/mnt/test_io -direct=1 -ioengine=libaio -group_reporting=1 -rw=randread -bs=128K -size=16G -numjobs=4 -iodepth=64  

后台执行可使用 nohup <command> &
```  
```
在windows中运行fio
fio -name=mytest -filename=e: -direct=1 -ioengine=windowsaio -thread=1 -group_reporting=1 -rw=randwrite -bs=4K -size=16G -iodepth=32 -runtime=300
```
- 若运行报错，安装libaio-devl
`yum install libaio-devel`

#### 3 参数和结果说明  
```
name=/mnt/test_io    #读写测试所在目录，通常选择需要测试的盘的data目录
direct=1             #测试过程绕过机器自带的buffer。使测试结果更真实
iodepth=64           #队列深度
rw=randwrite         #测试随机写的I/O
rw=randrw            #测试随机混合写和读的I/O
rw=randread          #测试随机读的I/O
rw=write             #测试顺序写的I/O
rw=read              #测试顺序读的I/O
rw=rw                #测试顺序混合写和读的I/O
ioengine=libaio      #io引擎使用libaio方式，异步I/O，减少交互次数，更有效率
bs=1024k             #单次io的块文件大小为1024k
size=16G              #本次测试的文件大小为16g，以每次1024k的io进行测试
numjobs=4            #本次测试的线程为4个
rwmixwrite=30        #在混合读写的模式下，写占30%
rwmixread=70         #在混合读写的模式下，读占70%
group_reporting      #关于显示结果的，汇总每个进程的信息  

Results Analysis
bw        #磁盘吞吐量
iops      #磁盘每秒读写次数
lat       #响应时延  
runtime   #测试时长
```
#### 4 一些注意事项
测试所需磁盘大小 = size * numjobs  
比如 16G * 4 = 64G，需要至少64G的磁盘空间  
可使用 `lsblk` 和 `df -h` 命令查看挂载的目录磁盘空间大小

---
### iPerf3
#### 1 下载地址  
[https://iperf.fr/iperf-download.php](https://iperf.fr/iperf-download.php)
#### 2 安装和运行  
（1）centOS yum 源直接安装 `yum install -y iperf3`    
（2）rpm 包安装 `rpm -ivh iperf3-xxx-xxx.x86_64.rpm`  
  
Server 端：`iperf3 -s`  
Client 端： `iperf3 -c <server_ipaddress>`  
#### 3 参数和结果说明  
[https://www.mankier.com/1/iperf3#Authors](https://www.mankier.com/1/iperf3#Authors)  
```
Server or Client:
  -p, --port      #         server port to listen on/connect to
  -f, --format    [kmgKMG]  format to report: Kbits, Mbits, KBytes, MBytes
  -i, --interval  #         seconds between periodic bandwidth reports
  -F, --file name           xmit/recv the specified file
  -B, --bind      <host>    bind to a specific interface
  -V, --verbose             more detailed output
  -J, --json                output in JSON format
  --logfile f               send output to a log file
  -d, --debug               emit debugging output
  -v, --version             show version information and quit
  -h, --help                show this message and quit

Server specific:
  -s, --server              run in server mode
  -D, --daemon              run the server as a daemon
  -I, --pidfile file        write PID file
  -1, --one-off             handle one client connection then exit

Client specific:
  -c, --client    <host>    run in client mode, connecting to <host>
  -u, --udp                 use UDP rather than TCP
  -b, --bandwidth #[KMG][/#] target bandwidth in bits/sec (0 for unlimited)
                            (default 1 Mbit/sec for UDP, unlimited for TCP)
                            (optional slash and packet count for burst mode)
  -t, --time      #         time in seconds to transmit for (default 10 secs)
  -n, --bytes     #[KMG]    number of bytes to transmit (instead of -t)
  -k, --blockcount #[KMG]   number of blocks (packets) to transmit (instead of -t or -n)
  -l, --len       #[KMG]    length of buffer to read or write
                            (default 128 KB for TCP, 8 KB for UDP)
  --cport         <port>    bind to a specific client port (TCP and UDP, default: ephemeral port)
  -P, --parallel  #         number of parallel client streams to run
  -R, --reverse             run in reverse mode (server sends, client receives)
  -w, --window    #[KMG]    set window size / socket buffer size
  -M, --set-mss   #         set TCP/SCTP maximum segment size (MTU - 40 bytes)
  -N, --no-delay            set TCP/SCTP no delay, disabling Nagle's Algorithm
  -4, --version4            only use IPv4
  -6, --version6            only use IPv6
  -S, --tos N               set the IP 'type of service'
  -Z, --zerocopy            use a 'zero copy' method of sending data
  -O, --omit N              omit the first n seconds
  -T, --title str           prefix every output line with this string
  --get-server-output       get results from server
  --udp-counters-64bit      use 64-bit counters in UDP test packets

[KMG] indicates options that support a K/M/G suffix for kilo-, mega-, or giga-
```
