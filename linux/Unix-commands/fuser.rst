.. index:: Fuser

Fuser
=======

fuser 命令用于显示正在访问指定文件,文件系统,或 unix socket 的进程 PID。
在默认显示模式下，每个文件名后跟一个表示访问类型的字母：

::

 c      当前目录.
 e      正在运行.
 f      打开文件。 默认显示模式中省略f。
 F      打开文件为了写入。 在默认显示模式下，F被省略。
 r      根目录.
 m      mmap的文件或共享库.

语法
----
``fuser [OPTIONS] name``

-a, --all 显示在命令行中指定的所有文件。 默认情况下，只显示至少一个进程访问的文件。

-k, --kill 杀死访问文件的进程。

-i, --interactive 在杀死一个进程之前请求用户确认。 如果-k不存在，则此选项将被忽略。

-l, --list-signals 列出所有已知的信号名称。

-m NAME, --mount NAME 显示使用指定的文件系统或块设备的所有进程

-M, --ismountpoint 仅当指定挂载点时，请求才会被执行，可以防止你杀死机器。

-w 仅杀死具有写权限的进程。 如果-k不存在，则此选项将被忽略。

-n SPACE, --namespace SPACE 选择一个不同的名字空间（file, udp, tcp）。

-s, --silent 安静模式

-SIGNAL 发送指定信号替代SIGKILL

-u, --user 将进程所有者的用户名追加到每个 PID 边上。

-v, --verbose 详细模式.




示例
----

显示访问了该目录的进程

::

 fuser -v /opt/jupyterlab/

::

                      USER        PID ACCESS COMMAND
 /opt/jupyterlab:     lfzyx      96365 ..c.. bash
                      root      123165 ..c.. sudo
                      root      123166 ..c.. su
                      root      123167 ..c.. bash
                      root      125752 ..c.. jupyterhub
                      root      125758 ..c.. node

显示访问了该文件系统或块设备的进程

::

 fuser -v -m /mnt
 fuser -v -m /dev/sdb1

::

                      USER        PID ACCESS COMMAND
 /dev/sdb1:           root      kernel mount /mnt
                      root       93433 F.... mount.ntfs
                      lfzyx     109588 ..c.. bash
                      lfzyx     110408 F.c.. md5sum
                      lfzyx     110421 F.c.. md5sum

fuser 命令也可用于检测使用网络端口的进程

::

 fuser -v -n tcp 8000

::

                      USER        PID ACCESS COMMAND
 8000/tcp:            root      125758 F.... node


杀死正在访问该文件系统或网络端口的进程

::

 fuser -k -m /mnt
 fuser -k  123/tcp
