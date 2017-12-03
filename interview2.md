### linux fork运行机制

question:代码如下

```cpp

#include "stdio.h"
#include "sys/types.h"
#include "unistd.h"
 
int main()
{
    pid_t pid1;
    pid_t pid2;
 
    pid1 = fork();
    pid2 = fork();
 
    printf("pid1:%d, pid2:%d\n", pid1, pid2);
}

```

已知从这个程序执行到这个程序的所有进程结束这个时间段内，没有其它新进程执行。

      1、请说出执行这个程序后，将一共运行几个进程。

      2、如果其中一个进程的输出结果是“pid1:1001, pid2:1002”，写出其他进程的输出结果（不考虑进程执行顺序）。

> 预备知识点

- 进程可以看作程序的一次执行过程，在linux下，每一个进程都有一个进程编号pid，为1-32768的整数，1是特殊进程init，其他进程从2开始，一直到32768，然后重新从2开始。

- Linux中用进程表结构来存储当前运行的进程，ps aux命令可以查看所有正在运行的进程。

- linux中的进程呈树状结构，init是进程的根结点。其他所有的进程都有父节点，生成这个进程的进程是这个进程的父进程。

- fork是复制与当前进程相同的进程，新的进程的所有数据都和父进程一直。

当执行fork函数的时候，可以将程序分成两个部分，以fork函数作为分割

![fork1](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview2/fork1.png)

分为四个步骤：

- 直接在初始进程P执行完partA。

- 执行到pid = fork()的时候，进程P启动了一个新的进程Q。和P是同一个程序的进程。Q继承P的所有变量、环境变量、程序计数器的当前值。

- 在进程P中，fork()启动新的进程,将Q的PID返回给pid，并继续执行PartB。

- 在进程Q中，fork()不启动新的进程且将0赋值给pid，并继续执行PartB（这里要注意，Q并没有执行PartA）。

接下来题目就好解了。

解题靠图
![](https://raw.githubusercontent.com/HenryHaoson/interView/master/images/interview2/fork2.png)

总共会产生3个新的进程，所以说打印语句会另外执行3次。


