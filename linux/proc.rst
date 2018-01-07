proc
=====

/proc 目录中的主要文件说明
apm	高级电源管理信息
cmdline	这个文件给出了内核启动的命令行
CPUinfo	中央处理器信息
devices	可以用到的设备（块设备/字符设备）
dma	显示当前使用的 DMA 通道
filesystems	核心配置的文件系统
ioports	当前使用的 I/O 端口
interrupts	这个文件的每一行都有一个保留的中断
kcore	系统物理内存映像
kmsg	核心输出的消息，被送到日志文件
mdstat	这个文件包含了由 md 设备驱动程序控制的 RAID 设备信息
loadavg	系统平均负载均衡
meminfo	存储器使用信息，包括物理内存和交换内存
modules	这个文件给出可加载内核模块的信息。lsmod 程序用这些信息显示有关模块的名称，大小，使用数目方面的信息
net	网络协议状态信息
partitions	系统识别的分区表
pci	pci 设备信息
scsi	scsi 设备信息
self	到查看/proc 程序进程目录的符号连接
stat	这个文件包含的信息有 CPU 利用率，磁盘，内存页，内存对换，全部中断，接触开关以及赏赐自举时间
swaps	显示的是交换分区的使用情况
uptime	这个文件给出自从上次系统自举以来的秒数，以及其中有多少秒处于空闲
version	这个文件只有一行内容，说明正在运行的内核版本。可以用标准的编程方法进行分析获得所需的系统信息