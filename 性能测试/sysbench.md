***[sysbench](https://github.com/Leanna-Lee/MyNotes/blob/master/%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/sysbench.md#sysbench)***   
- [CPU](https://github.com/Leanna-Lee/MyNotes/blob/master/%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/sysbench.md#1-cpu)   
- [Memory](https://github.com/Leanna-Lee/MyNotes/blob/master/%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/sysbench.md#2-memory)   
- [fileio](https://github.com/Leanna-Lee/MyNotes/blob/master/%E6%80%A7%E8%83%BD%E6%B5%8B%E8%AF%95/sysbench.md#3-fileio)   
# sysbench
sysbench is a scriptable multi-threaded benchmark tool based on LuaJIT. It is most frequently used for database benchmarks, but can also be used to create arbitrarily complex workloads that do not involve a database server.  
sysbench comes with the following bundled benchmarks:  
- oltp_*.lua: a collection of OLTP-like database benchmarks
- fileio: a filesystem-level benchmark
- cpu: a simple CPU benchmark
- memory: a memory access benchmark
- threads: a thread-based scheduler benchmark
- mutex: a POSIX mutex benchmark

https://github.com/akopytov/sysbench#sysbench  

### 安装
- RHEL/CentOS: `yum install sysbench`  
### Usage
`sysbench [options]... [testname] [command]`    
```    
Commands implemented by most tests: prepare run cleanup help  
General options:
  --threads=N                     number of threads to use [1]  
  --events=N                      limit for total number of events [0]  
  --time=N                        limit for total execution time in seconds [10]  
  --forced-shutdown=STRING        number of seconds to wait after the --time limit before forcing shutdown, or 'off' to disable [off]  
  --thread-stack-size=SIZE        size of stack per thread [64K]  
  --rate=N                        average transactions rate. 0 for unlimited rate [0]  
  --report-interval=N             periodically report intermediate statistics with a specified interval in seconds. 0 disables intermediate reports [0]  
  --report-checkpoints=[LIST,...] dump full statistics and reset all counters at specified points in time. The argument is a list of comma-separated values representing the amount of time in seconds elapsed from start of test when report checkpoint(s) must be performed. Report checkpoints are off by default. []  
  --debug[=on|off]                print more debugging info [off]  
  --validate[=on|off]             perform validation checks where possible [off]  
  --help[=on|off]                 print help and exit [off]  
  --version[=on|off]              print version and exit [off]  
  --config-file=FILENAME          File containing command line options  
  --tx-rate=N                     deprecated alias for --rate [0]  
  --max-requests=N                deprecated alias for --events [0]  
  --max-time=N                    deprecated alias for --time [0]  
  --num-threads=N                 deprecated alias for --threads [1]  

Pseudo-Random Numbers Generator options:
  --rand-type=STRING random numbers distribution {uniform,gaussian,special,pareto} [special]  
  --rand-spec-iter=N number of iterations used for numbers generation [12]  
  --rand-spec-pct=N  percentage of values to be treated as 'special' (for special distribution) [1]  
  --rand-spec-res=N  percentage of 'special' values to use (for special distribution) [75]  
  --rand-seed=N      seed for random number generator. When 0, the current time is used as a RNG seed. [0]  
  --rand-pareto-h=N  parameter h for pareto distribution [0.2]  

Log options:
  --verbosity=N verbosity level {5 - debug, 0 - only critical messages} [3]  

  --percentile=N       percentile to calculate in latency statistics (1-100). Use the special value of 0 to disable percentile calculations [95]  
  --histogram[=on|off] print latency histogram in report [off]  

General database options:

  --db-driver=STRING  specifies database driver to use ('help' to get list of available drivers) [mysql]  
  --db-ps-mode=STRING prepared statements usage mode {auto, disable} [auto]   
  --db-debug[=on|off] print database-specific debug information [off]    

Compiled-in database drivers:
  mysql - MySQL driver  
  pgsql - PostgreSQL driver  

mysql options:
  --mysql-host=[LIST,...]          MySQL server host [localhost]  
  --mysql-port=[LIST,...]          MySQL server port [3306]  
  --mysql-socket=[LIST,...]        MySQL socket  
  --mysql-user=STRING              MySQL user [sbtest]  
  --mysql-password=STRING          MySQL password []  
  --mysql-db=STRING                MySQL database name [sbtest]  
  --mysql-ssl[=on|off]             use SSL connections, if available in the client library [off]  
  --mysql-ssl-cipher=STRING        use specific cipher for SSL connections []  
  --mysql-compression[=on|off]     use compression, if available in the client library [off]  
  --mysql-debug[=on|off]           trace all client library calls [off]  
  --mysql-ignore-errors=[LIST,...] list of errors to ignore, or "all" [1213,1020,1205]  
  --mysql-dry-run[=on|off]         Dry run, pretend that all MySQL client API calls are successful without executing them [off]  

pgsql options:
  --pgsql-host=STRING     PostgreSQL server host [localhost]  
  --pgsql-port=N          PostgreSQL server port [5432]  
  --pgsql-user=STRING     PostgreSQL user [sbtest]  
  --pgsql-password=STRING PostgreSQL password []  
  --pgsql-db=STRING       PostgreSQL database name [sbtest]  

Compiled-in tests:
  fileio - File I/O test  
  cpu - CPU performance test  
  memory - Memory functions speed test  
  threads - Threads subsystem performance test  
  mutex - Mutex performance test  

See 'sysbench <testname> help' for a list of options for each test.
```  
#### 1 CPU
sysbench 的 CPU 测试是在指定时间内，循环进行素数计算。
```
# sysbench cpu help
sysbench 1.0.17 (using system LuaJIT 2.0.4)  

cpu options:  
  --cpu-max-prime=N upper limit for primes generator [10000]   

举个例子：  
# 素数生成上限 20000，2个线程，执行时间30秒
# sysbench cpu run --threads=2 --time=30 --cpu-max-prime=20000    

sysbench 1.0.17 (using system LuaJIT 2.0.4)  

Running the test with following options:  
Number of threads: 2  
Initializing random number generator from current time  


Prime numbers limit: 20000   # 每个线程每轮产生的素数上限均为2万个   

Initializing worker threads...  

Threads started!  

CPU speed:  
    events per second:   553.94   # 2个线程每秒完成了553.94轮计算 

General statistics:  
    total time:                          30.0032s  
    total number of events:              16621  

Latency (ms):  
         min:                                    3.51   # 完成1次event最小耗时3.51毫秒    
         avg:                                    3.61   # 完成1次event平均耗时3.61毫秒   
         max:                                   32.81   # 完成1次event最大耗时32.81毫秒 
         95th percentile:                        3.68   # 95%的events在3.68毫秒内完成
         sum:                                59976.97  

Threads fairness:
    events (avg/stddev):           8310.5000/22.50   #平均每个线程完成8310.5轮events，标准差22.50   
    execution time (avg/stddev):   29.9885/0.00   # 平均每个线程执行时间29.9885秒，标准差0
```  
>event：完成了 N 轮的素数计算  
stddev（标准差）：在相同时间内，多个线程分别完成的素数计算次数是否稳定。数值越低，表示多个线程的结果越接近（即越稳定）。该参数对于单线程无意义。  
- 相同时间，比较 event 数；
- 相同 event 数，比较时间；
- 时间和 event 数都相同，比较stddev（标准差）。
#### 2 Memory  
sysbench 的内存测试，以内存缓冲区 memory-block-siz 大小为单位，重复进行内存读或写操作，直到达到指定大小 memory-total-size。
```
# sysbench memory help  
sysbench 1.0.17 (using system LuaJIT 2.0.4)  

memory options:  
  --memory-block-size=SIZE    size of memory block for test [1K]  
  --memory-total-size=SIZE    total size of data to transfer [100G]  
  --memory-scope=STRING       memory access scope {global,local} [global]  
  --memory-hugetlb[=on|off]   allocate memory from HugeTLB pool [off]   # 是否使用大页，默认值为不使用   
  --memory-oper=STRING        type of memory operations {read, write, none} [write]  
  --memory-access-mode=STRING memory access mode {seq,rnd} [seq]   # 顺序读写、随机读写  
```
- when you execute a program the varaibles are allocated memory space. This memory is valid until the program is running. After the execution of the program this memory is releasd.   

- As the name says global memory is available across the programs. U can store something in global memory in one program and then read it from other program.   

- SAP memory is something above this ... Its is memory space allocated for all SAP applications and ABAP memory is memory allocated for ABAP objects like reports, function modules etc.   
```
举个例子：  
# 线程为 1，内存缓冲区大小 4KB，顺序写内存 2GB
sysbench memory run --threads=1 --memory-block-size=4K --memory-oper=write --memory-access-mode=seq --memory-total-size=2G   
sysbench 1.0.17 (using system LuaJIT 2.0.4)   

Running the test with following options:   
Number of threads: 1   
Initializing random number generator from current time   


Running memory speed test with the following options:   
  block size: 4KiB   
  total size: 2048MiB   
  operation: write   
  scope: global   

Initializing worker threads...   

Threads started!   

Total operations: 524288 (981794.44 per second)   # 内存顺序写操作总数 524288  

2048.00 MiB transferred (3835.13 MiB/sec)   # 内存顺序写带宽


General statistics:
    total time:                          0.5319s   
    total number of events:              524288   

Latency (ms):
         min:                                    0.00
         avg:                                    0.00
         max:                                    0.16
         95th percentile:                        0.00
         sum:                                  373.55   

Threads fairness:
    events (avg/stddev):           524288.0000/0.00   
    execution time (avg/stddev):   0.3736/0.00   
```   
>memory bandwidth = block size * operations   
4KiB * 524288 / 1024 = 2048MiB   
- block size 取值越大，内存带宽越大；   
- block size 达到某一值时，内存带宽达到物理上限。（block size 可尝试取值 8K，128K，512K，1M 等）   
#### 3 fileio
```
# sysbench fileio help   
sysbench 1.0.17 (using system LuaJIT 2.0.4)   

fileio options:   
  --file-num=N                  number of files to create [128]   
  --file-block-size=N           block size to use in all IO operations [16384]   
  --file-total-size=SIZE        total size of files to create [2G]   
  --file-test-mode=STRING       test mode {seqwr, seqrewr, seqrd, rndrd, rndwr, rndrw}   
  --file-io-mode=STRING         file operations mode {sync,async,mmap} [sync]   
  --file-async-backlog=N        number of asynchronous operatons to queue per thread [128]   
  --file-extra-flags=[LIST,...] list of additional flags to use to open files {sync,dsync,direct} []   
  --file-fsync-freq=N           do fsync() after this number of requests (0 - don't use fsync()) [100]   
  --file-fsync-all[=on|off]     do fsync() after each write operation [off]   
  --file-fsync-end[=on|off]     do fsync() at the end of test [on]   
  --file-fsync-mode=STRING      which method to use for synchronization {fsync, fdatasync} [fsync]   
  --file-merged-requests=N      merge at most this number of IO requests if possible (0 - don't merge) [0]   
  --file-rw-ratio=N             reads/writes ratio for combined test [1.5]   
```


