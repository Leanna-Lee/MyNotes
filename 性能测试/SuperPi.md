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
**下载地址**  

### Installation
```
# 解压
# tar -xvf supper_pi_src.tar
# 编译，64bit Linux修改Makefile，注释掉 CFLAGS += -march=i686 -malign-doubl
# cd pi_css5_src/
# make
gcc -Wall -pedantic -O -fomit-frame-pointer -funroll-loops pi_fftcs.o fftsg_h.o -lm -static -o pi_css5
# 运行

```
### Usage