Iotop
======

iotop是磁盘I/O实时监控器 [1]_

安装
----

``# apt-get install iostop``

语法
----

``# iotop ``\ ***``[OPTIONS]``***

←→改变排序

r 反向排序

-o 只显示正在执行I/O的进程和线程

-P 只显示进程

-p 只显示某个pid

-u 只显示某个用户

示例
----

只显示root用户的进程

``# iotop -u root -P``

| `` Total DISK READ:       0.00 B/s | Total DISK WRITE:       0.00 B/s``
| ``   PID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO>    COMMAND                                                                                                ``
| ``     1 be/4 root        0.00 B/s    0.00 B/s  0.00 %  0.00 % init [2]``
| `` 27943 be/4 root        0.00 B/s    0.00 B/s  0.00 %  0.00 % sshd: root@pts/1``
| `` 27945 be/4 root        0.00 B/s    0.00 B/s  0.00 %  0.00 % -bash``

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `Install Iotop (Monitor Linux Disk I/O) in RHEL, CentOS and
   Fedora <http://www.tecmint.com/install-iotop-monitor-linux-disk-io-in-rhel-centos-and-fedora/>`__
