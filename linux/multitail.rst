在单个shell中同时监控多个文件，算是tail的增强版

示例
----

同时监控两个文件

``# multitail /var/log/apache2/error.log /var/log/apache2/error.log.1``

分成两列监控两个文件

``# multitail -s 2 /var/log/mysqld.log /var/log/xferlog``

分成两列监控，第一列3个，第二列2个

``# multitail -s 2 -sn 3,2 /var/log/mysqld.log /var/log/xferlog /var/log/yum.log /var/log/ajenti.log /var/log/xferlog``

合并两个文件

``# multitail /var/log/mysqld.log -I /var/log/xferlog``

合并两个文件并且区分颜色

``# multitail -ci green /var/log/yum.log -ci yellow -I /var/log/mysqld.log``

监控文件的同时执行命令

``# multitail log/josei.access.log  -l "ping lfzyx.org"``

过滤内容

``# multitail -e "Opera/9.80" log/access.log ``

反选过滤内容

``# #multitail -v -e“ssh”/var/log/messages``
