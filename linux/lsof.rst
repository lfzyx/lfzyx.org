Lsof - 列出打开的文件
=======================

lsof 是 LiSt Open Files
的缩写，以列表的形式显示文件被哪个进程打开，文件包括磁盘文件、网络套接字、管道、设备。
使用这条命令的主要情形之一就是在无法卸载磁盘和显示正在使用或者打开某个文件的错误信息的时候。
使用这条命令，你可以很容易地看到正在使用哪个文件。 [#]_

语法
----

::

 lsof[-?abhlnNoOPRtUvVX] [+|-c c] [+|-d s] [+D D] [+|-f[gG]] [+|-e s] [-F [f]] [-g [s]] [-i [i]] 
 [+|-L [l]] [+m [m]] [+|-M] [-o [o]] [-p s] [+|-r [t]] [-s [p:s]] [-S [t]] [-T [t]] [-u s] [+|-w] [-x [fl]] [--] [names]

示例
----

列出所有打开的文件

::

 lsof

 COMMAND    PID      USER   FD (文件描述符)        TYPE（文件类型）     DEVICE  SIZE/OFF       NODE NAME
 init         1      root  cwd (当前工作目录)      DIR （目录）         253,0      4096          2 /
 init         1      root  rtd (root目录)          DIR （目录）         253,0      4096          2 /
 init         1      root  txt (程序代码和数据）   REG （普通文件）     253,0    145180     147164 /sbin/init
 init         1      root  mem (内存映射文件)      REG （普通文件）     253,0   1889704     190149 /lib/libc-2.12.so
 nginx     23780     nginx  0u（读写访问）         CHR （特殊字符文件） 1,3         0t0  188451717 /dev/null
 rsyslogd   3856     root   1r（读访问）           REG （普通文件）     144,8         0 4026545856 /proc/kmsg
 mysqld    32336     mysql  2w（写访问）           FIFO（先入先出）     0,6         0t0  126330358 pipe

列出指定进程打开的文件

::

 lsof -p 23780

 COMMAND   PID  USER   FD   TYPE             DEVICE SIZE/OFF      NODE NAME
 nginx   23780 nginx  cwd    DIR              144,7     4096 188448964 /
 nginx   23780 nginx  rtd    DIR              144,7     4096 188448964 /
 nginx   23780 nginx  txt    REG              144,7   825320 188454748 /usr/sbin/nginx
 nginx   23780 nginx  mem    REG                8,5          188454748 /usr/sbin/nginx (path dev=144,7)

列出指定用户打开的文件

::

 lsof -u nginx

 COMMAND   PID  USER   FD   TYPE             DEVICE SIZE/OFF      NODE NAME
 nginx   23780 nginx  cwd    DIR              144,7     4096 188448964 /
 nginx   23780 nginx  rtd    DIR              144,7     4096 188448964 /
 nginx   23780 nginx  txt    REG              144,7   825320 188454748 /usr/sbin/nginx
 nginx   23780 nginx  mem    REG                8,5          188454748 /usr/sbin/nginx (path dev=144,7)

列出所有网络连接

``lsof -i``

仅列出IPv4网络连接

``lsof -i 4``

仅列出TCP连接

``lsof -i TCP``

列出正在监听的网络连接

``lsof -i -s TCP:LISTEN``

列出已建立的网络连接

``lsof -i -s TCP:ESTABLISHED``

列出运行在指定端口上的进程

::

 lsof -i TCP:80

 COMMAND   PID  USER   FD   TYPE    DEVICE SIZE/OFF NODE NAME
 nginx   23780 nginx    3u  IPv4 338498973      0t0  TCP lfzyx.org:http->58.137.148.210:57582 (ESTABLISHED)
 nginx   23780 nginx    6u  IPv4    250008      0t0  TCP *:http (LISTEN)
 nginx   23780 nginx   19u  IPv4 338498376      0t0  TCP lfzyx.org:http->150-70-75-34.trendmicro.com:54366 (ESTABLISHED)
 nginx   23780 nginx   22u  IPv4 338498234      0t0  TCP lfzyx.org:http->wf-171-99-47-76.revip9.asianet.co.th:18447 (ESTABLISHED)
 nginx   32446  root    6u  IPv4    250008      0t0  TCP *:http (LISTEN)

列出运行在指定端口范围上的进程

::

 lsof -i TCP:1-1024

 COMMAND   PID   USER   FD   TYPE    DEVICE SIZE/OFF NODE NAME
 portmap  3759 daemon    5u  IPv4    249950      0t0  TCP *:sunrpc (LISTEN)
 nginx   23780  nginx    3u  IPv4 338529365      0t0  TCP lfzyx.org:http->ppp-124-121-2-252.revip2.asianet.co.th:64427 (ESTABLISHED)
 nginx   23780  nginx    6u  IPv4    250008      0t0  TCP *:http (LISTEN)
 nginx   32446   root    6u  IPv4    250008      0t0  TCP *:http (LISTEN)

列出到指定主机的连接

``lsof -i @10.10.10.118[:8080]``

列出除root用户打开的文件

``lsof -u ^root``

.. rubric:: 参考文献

.. [#] `10 lsof Command Examples in Linux <http://www.tecmint.com/10-lsof-command-examples-in-linux/>`_
