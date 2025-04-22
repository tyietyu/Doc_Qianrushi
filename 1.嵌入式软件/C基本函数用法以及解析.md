![2](./image/C基本函数用法以及解析.assets/2.png)

![3](./image/C基本函数用法以及解析.assets/3.png)

![4](./image/C基本函数用法以及解析.assets/4.png)

Sizeof 和strlen 的区别

1.sizeof是一个运算符，strlen是个函数，包含string.h

2.sizeof计算的是所占内存的大小，strlen计算的是字符串的长度。字符串以0结尾，\0是不需要计算长度的

操作寄存器的位数

\#define  SET(reg,n)    ((reg)=0x1<<(n)) n指定第几位为1