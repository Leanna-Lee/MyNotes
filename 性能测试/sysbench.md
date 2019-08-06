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
testname: fileio cpu memory threads mutex  
Commands implemented by most tests: prepare run cleanup help
#### 1 CPU

#### 2 Memory
#### 3 Fileio


