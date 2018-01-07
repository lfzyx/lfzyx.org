ss
=====


语法
----

-n 不解析服务名

-a 显示所有套接字

-l 只显示监听套接字

-p 显示使用该套接字的进程

-s 打印统计数据

-t 显示TCP套接字

-u 显示UDP套接字

-o state 列出指定状态的套接字

示例
----

``ss -4antp``

显示ipv4的TCP端口和使用它们的进程

``ss -4anup``

显示ipv4的UDP端口和使用它们的进程

``ss -4antp -o state established``

显示已经建立的连接
