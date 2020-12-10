### CPU 相关

`cat /proc/cupinfo`：查看 CPU 相关的基本信息

```bash
[root@localhost ~]# cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 158
model name      : Intel(R) Core(TM) i5-9400 CPU @ 2.90GHz
stepping        : 13
cpu MHz         : 2904.002
cache size      : 9216 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 22
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc eagerfpu pni pclmulqdq monitor ssse3 cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single fsgsbase avx2 invpcid rdseed clflushopt flush_l1d
bogomips        : 5808.00
clflush size    : 64
cache_alignment : 64
address sizes   : 39 bits physical, 48 bits virtual
power management:
```



`top`：

```bash
top - 07:49:20 up 5 min,  1 user,  load average: 0.00, 0.01, 0.01
Tasks:  93 total,   1 running,  92 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.0 sy,  0.0 ni,100.0 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  2914272 total,  2560228 free,   222864 used,   131180 buff/cache
KiB Swap:  3145724 total,  3145724 free,        0 used.  2542568 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
    1 root      20   0  128012   6596   4140 S  0.0  0.2   0:00.86 systemd
    2 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kthreadd
    4 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kworker/0:0H
    5 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kworker/u2:0
    6 root      20   0       0      0      0 S  0.0  0.0   0:00.02 ksoftirqd/0
    7 root      rt   0       0      0      0 S  0.0  0.0   0:00.00 migration/0
    8 root      20   0       0      0      0 S  0.0  0.0   0:00.00 rcu_bh
    9 root      20   0       0      0      0 S  0.0  0.0   0:00.25 rcu_sched
   10 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 lru-add-drain
   11 root      rt   0       0      0      0 S  0.0  0.0   0:00.00 watchdog/0
   13 root      20   0       0      0      0 S  0.0  0.0   0:00.00 kdevtmpfs
   14 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 netns
   15 root      20   0       0      0      0 S  0.0  0.0   0:00.00 khungtaskd
   16 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 writeback
   17 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 kintegrityd
   18 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 bioset
   19 root       0 -20       0      0      0 S  0.0  0.0   0:00.00 bioset
   

# 其中，
```

测试系统负载，参考命令：

```bash
{ yes > /dev/null & } && sleep 30 && ps -ef | grep yes | awk '{print $2}' | xargs kill
```

```bash
for i in $( seq 0 $(($(cat /proc/cpuinfo | grep processor | wc -l)-1)) );do taskset -c $i yes > /dev/null & done && sleep 30 && ps -ef | grep yes | awk '{print $2}' | xargs kill
```

常用方式：

- `top -p 进程号`：只观察一个进程的状态

  ```bash
  $ top -p 732
  
  top - 22:35:00 up 42 days, 11:16, 28 users,  load average: 0.00, 0.01, 0.05
  Tasks:   0 total,   0 running,   0 sleeping,   0 stopped,   0 zombie
  %Cpu(s):  0.2 us,  0.5 sy,  0.0 ni, 99.3 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
  KiB Mem :  3881920 total,   702676 free,   302076 used,  2877168 buff/cache
  KiB Swap:        0 total,        0 free,        0 used.  1184568 avail Mem
  
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
  
  ```

- `top -p 进程号 -d 间隔时间 -n 刷新次数`

- 其他：

  - `-b`：批处理模式



### 关于内存

`free`

```bash
[root@localhost ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:           2845         211        2506           8         127        2488
Swap:          3071           0        3071

[root@localhost ~]# free -h  # 更便于阅读
              total        used        free      shared  buff/cache   available
Mem:           2.8G        211M        2.4G        8.5M        127M        2.4G
Swap:          3.0G          0B        3.0G
```



### 关于 IO

`iostat`（需要安装：`yum install sysstat`）

- `-c`：只看 CPU
- `-d`：只看硬盘

```bash
[root@localhost ~]# iostat 1  # 1秒刷新一次
Linux 3.10.0-1160.el7.x86_64 (localhost.localdomain)    2020年12月10日  _x86_64_        (1 CPU)

avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           0.26    0.00    0.32    0.05    0.00   99.36

Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn
sda               5.06       181.55        18.64     244793      25135
dm-0              3.43       157.87        17.12     212867      23087
dm-1              0.07         1.63         0.00       2204          0
```

`iftop`：类似于top的实时流量监控工具（需要安装才能使用）



![image-20201210230231291](C:\Users\xiaoj\AppData\Roaming\Typora\typora-user-images\image-20201210230231291.png)