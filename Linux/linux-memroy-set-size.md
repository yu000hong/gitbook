# 内存：VSS/RSS/PSS/USS

`VSS` - Virtual Set Size: 虚拟耗用内存（包含共享库占用的内存）

`RSS` - Resident Set Size: 实际使用物理内存（包含共享库占用的内存）

`PSS` - Proportional Set Size: 实际使用的物理内存（比例分配共享库占用的内存）

`USS` - Unique Set Size: 进程独自占用的物理内存（不包含共享库占用的内存）

一般来说内存占用大小有如下规律：VSS >= RSS >= PSS >= USS

> `VSS` (reported as `VSZ` from ps) is the total accessible address space of a process. This size also includes memory that may not be resident in RAM like mallocs that have been allocated but not written to. **VSS is of very little use** for determing real memory usage of a process.

> `RSS` is the total memory actually held in RAM for a process. **RSS can be misleading**, because it reports the total all of the shared libraries that the process uses, even though a shared library is only loaded into memory once regardless of how many processes use it. RSS is not an accurate representation of the memory usage for a single process.

> `PSS` differs from RSS in that it reports the proportional size of its shared libraries, i.e. if three processes all use a shared library that has 30 pages, that library will only contribute 10 pages to the PSS that is reported for each of the three processes. **PSS is a very useful number** because when the PSS for all processes in the system are summed together, that is a good representation for the total memory usage in the system. When a process is killed, the shared libraries that contributed to its PSS will be proportionally distributed to the PSS totals for the remaining processes still using that library. In this way PSS can be slightly misleading, because when a process is killed, PSS does not accurately represent the memory returned to the overall system.

> `USS` is the total private memory for a process, i.e. that memory that is completely unique to that process. **USS is an extremely useful number** because it indicates the true incremental cost of running a particular process. When a process is killed, the USS is the total memory that is actually returned to the system. USS is the best number to watch when initially suspicious of memory leaks in a process.

### How to check memory usage per process in Linux

**ps**

```
# ps -eo pid,tid,class,rtprio,stat,vsz,rss,comm
  PID   TID CLS RTPRIO STAT    VSZ   RSS COMMAND
 1283  1283 TS       - S<sl  59636  1116 auditd
 1337  1337 TS       - Ssl  476848  9996 NetworkManager
 1366  1366 TS       - Ssl  553488 13196 polkitd
 1456  1456 TS       - S    267684 31768 sssd_nss
 1617  1617 TS       - Ss+  116376   872 agetty
 2955  2955 TS       - Ss    93728  2236 master
 4843  4843 TS       - Ss   156356  5156 sshd
```

> VSZ: VSS in ps
>
> RSS: RSS in ps

**top**

```
# top
Tasks: 717 total,   1 running, 716 sleeping,   0 stopped,   0 zombie
%Cpu(s):  6.1 us,  2.9 sy,  0.0 ni, 90.8 id,  0.0 wa,  0.0 hi,  0.3 si,  0.0 st
KiB Mem : 65694756 total,  1988968 free, 23250312 used, 40455476 buff/cache
KiB Swap:  8388604 total,     6076 free,  8382528 used. 39705912 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
13608 10001     20   0 3514.3m 428.7m   9.4m S   6.0  0.7   6296:57 relay
28738 systemd+  20   0  659.4m  42.7m   4.9m S   5.3  0.1   2457:19 snuba
 1170 104       20   0 6528.1m 171.4m   2.5m S   4.0  0.3 380:50.73 beam.smp
27396 polkitd   20   0  384.7m 118.5m   3.1m S   2.6  0.2  14:07.15 python3
17703 systemd+  20   0  264.8m  68.3m   6.7m S   2.0  0.1 331:39.64 daphne
```

> VIRT: VSS in top
>
> RES: RSS in top
