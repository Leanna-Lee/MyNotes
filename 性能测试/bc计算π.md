# bc 计算 π
bc 命令是任意精度计算器语言，通常在linux下当计算器用。它类似基本的计算器，使用这个计算器可以做基本的数学运算。
- 常用的运算：
```
+ 加法
- 减法
* 乘法
/ 除法
^ 指数
% 余数
```
### Installation
- RHEL/CentOS: `yum install bc` 
### Usage
```
# bc --help
usage: bc [options] [file ...]
  -h  --help         print this usage and exit
  -i  --interactive  force interactive mode
  -l  --mathlib      use the predefined math routines   //使用标准数学库
  -q  --quiet        don't print initial banner
  -s  --standard     non-standard bc constructs are errors
  -w  --warn         warn about non-standard bc constructs
  -v  --version      print version information and exit
```
- 举个例子   
```
# echo '1+2+3+4+5' | bc   
15
# echo '2 ^ 10' | bc    //计算2的10次方
1024
# echo 'scale=5; 13/19' | bc   //精度为5
.68421
# echo 'scale=6; (1+2+3-4)*5/6^2' | bc   //精度为6
.277777
# 计算小数点后一万位π
# echo "scale=10000; 4*a(1)" | bc -l   

# 统计计算时间，且指定在某一个CPU core上运行程序
# time echo "scale=10000; 4*a(1)" | taskset -c 1 bc -l
```
- a(x): The arctangent of x, arctangent returns radians.  
**反正切函数，tan(π/4)=1，所以arctan(1)=π/4，所以4*a(1)=π**