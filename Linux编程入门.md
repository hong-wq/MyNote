[TOC]

# Linux编程入门

## GCC

GCC:GNU C语言编译器

### 发展

<img src="E:\Note\图片\gcc发展.PNG" style="zoom: 50%;" />

### 工作流程

<img src="E:\Note\图片\gcc工作.PNG" style="zoom: 50%;" />

### linux指令

<img src="E:\Note\图片\gcc参数.PNG" style="zoom: 67%;" />

注意都是E和S都是大写

**1.预处理**

gcc test.c -E -o test.i

- 删除注释
- 宏替换
- 展开头文件

**2.编译**

生成汇编语言

gcc test.i -S -o test.s

**3.汇编**

gcc test.i -c test.o

**4.链接**

* 目标代码：汇编器生成的二进制代码(可有多个)
* 启动代码：程序与操作系统之间的接口
* 库代码：如printf()的实体代码

链接生成可执行文件.exe.out

gcc test.o -o test.out



## 库

分为两大类

* 静态库
* 动态库

 静态库：GCC 进行链接时，会把静态库中代码打包到可执行程序中 

 动态库：GCC 进行链接时，动态库的代码不会被打包到可执行程序中，动态加载

主要区别是在链接阶段如何处理

### 静态库.a

**制作：**

![](E:\Note\图片\静态库.PNG)

**1.获取.o文件**

gcc -c 指令

**2. .o文件打包**

使用ar工具

ar rcs libxxx.a xxx.o xxx.o（注意包名要加lib）



### 动态库.so

**制作**

![image-20220408144130117](C:\Users\QWQ\AppData\Roaming\Typora\typora-user-images\image-20220408144130117.png)

**1.获取与位置无关.o文件**

gcc -c -fpic a.c b.c

**2.动态库合成**

gcc -shared a.o b.o c.o -o libxxx.so

**3.使用**

1.动态库是动态加载到内存中，需要动态获取

2.ldd+可执行文件：查看动态库依赖关系

3.需要使用动态载入器获取绝对路径

![](E:\Note\图片\动态库路径.PNG)

加入环境变量：**export  LD_LIBRARY_PATH=$ LD_LIBRARY_PATH:路径**

可修改两个文件

*  ~/.bashrc
* /etc/profile





## 文件IO

### 标准C库IO与Linux系统IO

* 标准C库的IO函数(高级)
* Linux系统的IO函数(低级、底层)

两者之间的关系为**调用**与**被调用**

**标准C库：**

* 查看文档man 3 funtion()

- 跨平台（Linux，window，mac都可使用）

Linux使用fopen()就使用Linux的系统API（open）打开文件

**Linux系统**

* 产看文档man 2 funtion

**跨平台实现方式两种:**

1.为不同平台设置虚拟机，如java

2.封装时针对不同平台，调用不同的系统API

<img src="E:\Note\图片\IO函数.PNG" style="zoom:67%;" />

fopen()打开一个文件时，调用系统API(open)返回一个文件指针FILE *fp

**FILE *fp**

一个结构体，包含3个重要的部分

* 文件描述符

  索引文件的位置，定位

* 文件读写指针

  读指针与写指针实际位置

* IO缓冲区

  就是在内存中

  把数据从内存写入磁盘（缓冲区满了，刷新、文件关闭）

  可提高运行效率

<img src="E:\Note\图片\IO关系.PNG" style="zoom:67%;" />





### 虚拟地址空间

<img src="E:\Note\图片\虚拟地址空间.PNG" style="zoom:67%;" />



* 虚拟不存在

* 大小由内存决定

  32位有4G,$2^{32}$

* 被CPU里的MMU映射到真实物理内存

* 用户只可管理用户区，对内核区无权限

* 操作内核区只能进行系统调用API实现

### 文件描述符

<img src="E:\Note\图片\文件描述符.PNG" style="zoom:67%;" />



* 保存在**内核区**内存管理模块的PCB中(内核就是一个程序)

* 存储的结构为数组：文件描述表

  大小默认1024

  前三个默认被占用且打开

  标准输入0、输出1、错误2，对应于当前使用终端(终端也是一个文件)

* 一个文件被打开多次文件描述符是不一样的

* 每当打开一个新文件，文件描述符都是取最小(由内核管理)



### open()函数

**1.int open(const char *pathname, int flags);**

```c
/*
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
打开一个已经存在的文件
int open(const char *pathname, int flags);
    参数：
        -- pathname:文件路径
        -- flags:文件的权限操作(只读、只写、可读可写)及其他设置
        O_RDONLY、O_WRONLY、O_RDWR  
    返回值:
        -- 成功：文件描述符
        -- 失败： -1
errno:Linux系统库里的一个全局变量,记录错误号
perror:通过error打印错误描述,属于标C库<stdio.h>
        void perror(const char *s);
*/
//打开一个不存在的文件
int main()
{
    int fd=open("a.txt",O_RDONLY);
    if(fd=-1)
    {
        perror("open");//包含在stdio.h
    }
    close(fd);//包含在unistd.h
    return 0;
    
}
//输出结果：open: No such file or directory


```

**2.int open(const char \*pathname, int flags, mode_t mode);**

```c
/*
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
创建一个新文件
int open(const char *pathname, int flags, mode_t mode);
    参数：
        -- pathname:文件路径
        -- flags:文件的权限操作(只读、只写、可读可写)及其他设置
                必选项：O_RDONLY、O_WRONLY、O_RDWR  互斥
                可选项：O_CREATE
        -- mode:八进制数，表示新创建出文件的操作权限,如给定0777(最高权限)
            最终权限为:mode & ~umask(umask指令查看)
            0777 ->111111111
        &   0775 ->111111101
           ----------------------
                   111111101
          mask作用就是给文件抹去某些权限(&有0就为0了)
          
    返回值:
        -- 成功：文件描述符
        -- 失败： -1
        
    flags参数为什么要按位或？
    	flag为int 类型，4字节，32位
    	1.rd,2.wr,3,rdwr,4.create
    	|有1就为1,因此可以加上某些权限。
    	
*/

#include<sys/types.h>
#include<sys/stat.h>
#include<fcntl.h>
#include<stdio.h>
#include<unistd.h>
int main()
{
    int fd=open("createa.txt",O_RDWR|O_CREAT,0777);
    if(fd==-1)
    {
        perror("create");
    }
    close(fd);
    return 0;
    
}
```





































