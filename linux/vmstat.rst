vmstat 对系统的进程情况、内存使用情况、交换页和 I/O 块使用情况、
中断以及 CPU 使用情况进行统计并报告相应的信息。 [1]_

语法
----

``vmstat ``\ ***``[options]``\ ````\ ``[delay``\ ````\ ``[count]]``***

-a 列出活动/非活动内存

-s 统计事件计数器和内存信息

-d 磁盘的统计数据

-S 设置字节单位

delay 间隔

count 次数

示例
----

``# vmstat -S M 2 5``

使用MB单位，每2秒执行一次，共执行5次

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `Linux Performance Monitoring with Vmstat and Iostat
   Commands <http://www.tecmint.com/linux-performance-monitoring-with-vmstat-and-iostat-commands/>`__
