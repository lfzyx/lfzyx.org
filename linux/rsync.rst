Rsync - 文件同步
=================

`Rsync <http://rsync.samba.org/documentation.html>`_  是一个开源实用程序，提供快速的增量文件传输，
并适当利用差分编码以减少数据传输量。在 daemon
模式下，rsync监听873端口，通过rsync协议传输文件。在ssh模式下，通过ssh协议传输文件。

安装
----

``apt-get install rsync``

配置
----

服务端
~~~~~~

::

 vi /etc/rsyncd.conf

 #motd file=/etc/motd
 log file=/var/log/rsyncd.log
 pid file=/var/run/rsyncd.pid
 #syslog facility=daemon
 #socket options=
 [ftp]
        comment = public archive
        path = PATH
        use chroot = yes
 #      max connections=10
        lock file = /var/lock/rsyncd
        read only = no
        list = yes
        uid = nobody 和目录属主保持一致
        gid = nogroup 和目录属组保持一致
 #      exclude =
 #      exclude from =
 #      include =
 #      include from =
        auth users =
        secrets file = /etc/rsyncd.secrets
        strict modes = yes
        hosts allow = ip地址
 #      hosts deny =
        ignore errors = no
        ignore nonreadable = yes
        transfer logging = no
 #      log format = %t: host %h (%a) %o %f (%l bytes). Total %b bytes.
        timeout = 600
        refuse options = checksum dry-run
        dont compress = *.gz *.tgz *.zip *.z *.rpm *.deb *.iso *.bz2 *.tbz

::

 vi /etc/rsyncd.secrets
 user:userpasswd

::

 chmod 600 /etc/rsyncd.secrets
 service rsync start

客户端
~~~~~~
::

 vi /etc/rsyncd.secrets
 userpasswd

::

 chmod 600 /etc/rsyncd.secrets

语法
----

::

 本地同步:
 rsync [OPTION...] SRC... DEST

::

 通过远程 ssh:
 Pull:
 rsync [OPTION...] [USER@]HOST:SRC... DEST
 Push:
 rsync [OPTION...] SRC... [USER@]HOST:DEST

::

 通过 rsync 服务器:
 Pull:
 rsync [OPTION...] [USER@]HOST::SRC... DEST
 rsync [OPTION...] rsync://[USER@]HOST[:PORT]/SRC... DEST
 Push:
 rsync [OPTION...] SRC... [USER@]HOST::DEST
 rsync [OPTION...] SRC... rsync://[USER@]HOST[:PORT]/DEST

-r 递归地同步目录

-l 保持符号链接文件

-p 保持文件权限

-t 保持文件时间信息

-g 保持文件属组信息
（如果运行rsync操作的不是超级用户，则仅当调用rsync操作的用户属于源文件属组成员时，才保持属组信息）

-o 保持文件属主信息 (super-user only)

-D 保持设备文件和特殊文件 (super-user only)

-a 归档模式，相当于 -rlptgoD

-v 输出详情

-z 开启压缩

--compress-level=NUM 明确设置压缩级别

-P 显示进度和断点传续

-e 使用ssh和ssh语法

-H 保持文件的硬链接特性

-L 遇到符号链接时，同步该符号链接所指向的文件，而不是该符号链接

-u 排除被修改过的目的文件

--delete 在DEST删除SRC没有的文件

