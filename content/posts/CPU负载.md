---
title: "CPU负载"
date: 2020-05-27T00:16:27+08:00
draft: false
---


### 查看 CPU 信息

#### 物理 cpu 数（physical cpu）

> 指主板上实际插入的cpu硬件个数（socket)

#### 核心（core）

> CPU的核心数是指物理上，也就是硬件上存在着几个核心。比如,双核就是包括2个相对独立的CPU核心单元组，四核就包含4个相对独立的CPU核心单元组


#### 同时多线程技术（simultaneous multithreading）和 超线程技术（hyper–threading/HT）

    >> 简单地说，就是模拟出的CPU核心数。比如，可以通过一个CPU核心数模拟出2线程的CPU，也就是说，这个单核心的CPU被模拟成了一个类似双核心CPU的功能。我们从任务管理器的性能标签页中看到的是两个CPU。

### 查看 cpu 信息命令

查看物理 cpu 数：

`cat /proc/cpuinfo| grep "physical id"| sort| uniq| wc -l`

查看每个物理 cpu 中 核心数(core 数)：

`cat /proc/cpuinfo | grep "cpu cores" | uniq`

查看总的逻辑 cpu 数（processor 数）：

`cat /proc/cpuinfo| grep "processor"| wc -l`
查看 cpu 型号：

`cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c`
判断 cpu 是否 64 位：

`检查 cpuinfo 中的 flags 区段，看是否有 lm （long mode） 标识`




### 平均负载于 CPU 使用率

###平均负载的含义上来，平均负载是指单位时间内，处于可运行状态和不可中断状态的进程数。所以，它不仅包括了正在使用
"cat /proc/cpuinfo"命令，可以查看CPU信息。"grep -c 'model name' /proc/cpuinfo"命令，直接返回CPU的总核心数。
当系统负荷持续大于0.7，你必须开始调查了，问题出在哪里，防止情况恶化。
当系统负荷持续大于1.0，你必须动手寻找解决办法，把这个值降下来。




### CPU 使用率
CPU 的进程，还包括等待 CPU 和等待 I/O 的进程##
而 CPU 使用率CPU利用率是指CPU工作时间占总时间的比重，
公式如下：
`Utilization= work_time/work_time+idle_ime`
 可见，总时间由一段连续时间内的CPU工作时间长度和CPU空闲时间长度组成。

### 区别
比如：CPU 密集型进程，使用大量 CPU 会导致平均负载升高，此时这两者是一致的；
I/O 密集型进程，等待 I/O 也会导致平均负载升高，但 CPU 使用率不一定很高；
大量等待 CPU 的进程调度也会导致平均负载升高，此时的 CPU 使用率也会比较高。






### top命令详解
```
%Cpu(s):  0.8 us,  0.7 sy,  0.0 ni, 98.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
```

```
us, user    : time running un-niced user processes
sy, system  : time running kernel processes
ni, nice    : time running niced user processes
id, idle    : time spent in the kernel idle handler
wa, IO-wait : time waiting for I/O completion
hi : time spent servicing hardware interrupts
si : time spent servicing software interrupts
st : time stolen from this vm by the hypervisor
```






### CPU 上下文切换
1. CPU 上下文: 寄存器 , 计数器的快照. 也就是 cpu 的工作环境
2. 切换发生的场景
    1. 特权模式 (系统调用)
        * CPU特权模式切换就是用户态和内核态的切换，用户态进程通过调用系统调用进入到内核态，
    2. 进程上下文切换
        进程上下文切换是不可避免的，通常情况下，进程的数量一定是远远超过CPU核心数的，进程不能独占CPU，需要接受内核的调度，每次调度都会触发进程切换。
        引发内核重新调度的因素有：
           * 正在运行的进程的当前时间片耗尽
           * 系统资源不足（譬如内存），进程需要等待
           * 进程通过sleep等函数，将自己挂起
           * 出现了优先级更高的进程（内核需要设置为支持抢占）
           * 发生硬件中断，被切换到中断服务程序（注意，软中断不会触发切换）
    3. 线程上下文切换
        隶属于同一个继承的线程间进行上下文切换的开销相对较小，因为它们有一部分共用的数据，这些数据可以免除切换。
        所谓内核中的任务调度，实际上的调度对象是线程；而进程只是给线程提供了虚拟内存、全局变量等资源
    4. 中断上下文切换
       *.  为了快速响应硬件的事件，中断处理会打断进程的正常调度和执行，转而调用中断处理程序，响应设备事件。而在打断其他进程时，就需要将进程当前的状态保存下来，这样在中断结束后，进程仍然可以从原来的状态恢复运行。
### 分析 CPU 上下文切换的工具
    1. vmstat
       *  `vmstat
        是一个常用的系统性能分析工具，主要用来分析系统的内存使用情况，也常用来分析
        vmstat 只给出了系统总体的上下文切换情况
        CPU 上下文切换和中断的次数。`
        ```
         cs（context switch）是每秒上下文切换的次数。
         in（interrupt）则是每秒中断的次数。
         r（Running or Runnable）是就绪队列的长度，也就是正在运行和等待 CPU 的进程数。
         b（Blocked）则是处于不可中断睡眠状态的进程数。
        ```
    2. pidstat
        1. cswch (voluntary context switches)
            * 所谓自愿上下文切换，是指进程无法获取所需资源，导致的上下文切换。比如说， I/O、内存等系统资源不足时，就会发生自愿上下文切换。
        2, nvcswch (non voluntary context switches)
            *
            而非自愿上下文切换，则是指进程由于时间片已到等原因，被系统强制调度，进而发生的上下文切换。比如时间片到了

 ### 案例分析
    1. r 进程就绪队列和 cpu core 的对比, 远超过 cpu 核心数量,
       判断是等待进程过多.
    2. 使用 vmstat 的 cs 上下文切换看总体上下文切换, in 中断次数
    3. pidstat -w -u 1, 判断具体让 CPU 升高的进程
    4, pidstat -wt 1, 输出线程的 cpu 治疗
    5. `/proc/interrupts`

### CPU 使用率
    1. top 显示总进程使用率
    2. pidstat 更加细化
        * 用户态 CPU 使用率 （%usr）；
            * 说明用户态进程占用了较多的 CPU，所以应该着重排查进程的性能问题。
        * 内核态 CPU 使用率（%system）；
            * 所以应该着重排查内核线程或者系统调用的性能问题。
        * 运行虚拟机 CPU 使用率（%guest）；
        * 等待 CPU 使用率（%wait）；
        * 以及总的 CPU 使用率（%CPU）。
    3. perf [process], 找到具体占用 cpu 函数


#### 案例分析
    1. top 检查总体 cpu 使用率, 发现大量 runine , 和 sleep 的进程
    2. pidstat 查个体 cpu 使用率, 看进程的 s 列表的状态
    3. 碰到常规问题无法解释的 CPU 使用率情况时，首先要想到有可能是短时应用导致的问题，比如有可能是下面这两种情况。第一，应用里直接调用了其他二进制程序，这些程序通常运行时间比较短，通过 top 等工具也不容易发现。第二，应用本身在不停地崩溃重启，而启动过程的资源初始化，很可能会占用相当多的 CPU。对于这类进程，我们可以用 pstree 或者 execsnoop 找到它们的父进程，再从父进程所在的应用入手，排查问题的根源。


#### 僵尸进程解决办法
    孤儿进程：一个父进程退出，而它的一个或多个子进程还在运行，那么那些子进程将成为孤儿进程。孤儿进程将被init进程(进程号为1)所收养，并由init进程对它们完成状态收集工作。
    ** 没有危害**
    僵尸进程：一个进程使用fork创建子进程，如果子进程退出，而父进程并没有调用wait或waitpid获取子进程的状态信息，那么子进程的进程描述符仍然保存在系统中。这种进程称之为僵死进程。
    会占用大量 pid , 而导致系统没有足够的进程号码产生新的进程

    top 的 S (STATUS) 列很重要
    **R** 是 Running 或 Runnable 的缩写，表示进程在 CPU 的就绪队列中，正在运行或者正在等待运行
    **D** 是 Disk Sleep 的缩写，也就是不可中断状态睡眠（Uninterruptible Sleep），一般表示进程正在跟硬件交互，并且交互过程不允许被其他进程或中断打断
    **Z** 是 Zombie 的缩写，如果你玩过“植物大战僵尸”这款游戏，应该知道它的意思它表示僵尸进程，也就是进程实际上已经结束了，但是父进程还没有回收它的资源（比如进程的描述符、PID 等）
    **S** 是 Interruptible Sleep 的缩写，也就是可中断状态睡眠，表示进程因为等待某个事件而被系统挂起
    当进程等待的事件发生时，它会被唤醒并进入 R 状态
    **I** 是 Idle 的缩写，也就是空闲状态，用在不可中断睡眠的内核线程上
    前面说了，硬件交互导致的不可中断进程用 D 表示，但对某些内核线程来说，它们有可能实际上并没有任何负载，用 Idle 正是为了区分这种情况
    要注意，D 状态的进程会导致平均负载升高， I 状态的进程却不会

    1. top 查看 cpu 负载, iowait, 队列数量, cpu 使用率,
       然后最后看每个线程具体的状态
    2. dstat 1 10 查看 iowait 的 整体情况
    3. pidstat -d 1 20 查看具体进程读写情况
    4. 查看单个进程情况. `ps aux | grep 6082`
    5. perf [process]
