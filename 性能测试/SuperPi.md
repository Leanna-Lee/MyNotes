# Super Pi
Super PI is a single threaded benchmark that calculates pi to a specific number of digits. It uses the Gauss-Legendre algorithm.
```
# Installation
# tar -xvf superpi_linux.tar.gz
superpi/
superpi/pi
superpi/super_pi

# 计算2的20次方位 π 所需时间
# ./super_pi 20
```
- 使用 mpstat 命令查看 CPU 运行状态，可以看到 super_pi 计算圆周率时，会占用部分内核空间，且计算时间较长。   
## 以下为改进版计算 π 算法： 
This is a modified version of Gauss-Legendre formula (by T.Ooura). It is faster than original version, calculating PI(= 3.14159...) using FFT and AGM.   
   
**下载地址**  
https://download.csdn.net/download/weixin_41647043/11490791

### Installation / Usage
```
# 解压
# tar -xvf supper_pi_src.tar
# 编译，64bit Linux修改Makefile，注释掉 CFLAGS += -march=i686 -malign-doubl
# cd pi_css5_src/
# make
gcc -Wall -pedantic -O -fomit-frame-pointer -funroll-loops pi_fftcs.o fftsg_h.o -lm -static -o pi_css5
# 运行
# ./pi_css5 $((1<<20))   //计算2的20次方位π
# ./pi_css5 $((1<<25))   //计算2的25次方位π
```   
使用mpstat查看，此程序在运行时，CPU 用户空间占用100%，内核空间占用基本为0，符合需求。   
```
# mpstat -P ALL 1   //查看所有CPUs的运行情况，每1秒更新一次

10时50分10秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
10时50分11秒  all   50.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00   50.00
10时50分11秒    0    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00
10时50分11秒    1  100.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00

10时50分11秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
10时50分12秒  all   50.25    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00   49.75
10时50分12秒    0    1.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00   99.00
10时50分12秒    1  100.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00

10时50分12秒  CPU    %usr   %nice    %sys %iowait    %irq   %soft  %steal  %guest  %gnice   %idle
10时50分13秒  all   49.75    0.00    0.50    0.00    0.00    0.00    0.00    0.00    0.00   49.75
10时50分13秒    0    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00  100.00
10时50分13秒    1  100.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00    0.00
```