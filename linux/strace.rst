`strace <http://sourceforge.net/projects/strace/>`__\ 是\ `Linux环境下的一款程序调试工具 <Linux>`__\ ，用来跟踪进程执行时的系统调用和所接收的信号。

安装
----

编译
~~~~

| ```wget`` <wget>`__\ `` ``\ ```http://ncu.dl.sourceforge.net/project/strace/strace/4.8/strace-4.8.tar.xz`` <http://ncu.dl.sourceforge.net/project/strace/strace/4.8/strace-4.8.tar.xz>`__\  [1]_
| ```tar`` <tar>`__\ `` xvf strace-4.8.tar.xz``
| ```cd`` <cd>`__\ `` strace-4.8``
| ``./configure``
| ``make``
| ``make install``

apt
~~~

```apt-get`` <apt-get>`__\ `` install strace``

参数
----

-a column 设置返回值的输出位置.默认 为40.

-e expr 指定一个表达式,用来控制如何跟踪.格式如下:

[qualifier=][!]value1[,value2]... qualifier只能是
trace,abbrev,verbose,raw,signal,read,write其中之一.value是用来限定的符号或数字.默认的
qualifier是 trace.感叹号是否定符号.例如:

-eopen等价于 -e
trace=open,表示只跟踪open调用.而-etrace!=open表示跟踪除了open以外的其他调用.有两个特殊的符号
all 和 none. 注意有些shell使用!来执行历史记录里的命令,所以要使用\\.

-e trace=set 只跟踪指定的系统 调用.例如:-e
trace=open,close,rean,write表示只跟踪这四个系统调用.默认的为set=all.

-e trace=file 只跟踪有关文件操作的系统调用.

-e trace=process 只跟踪有关进程控制的系统调用.

-e trace=network 跟踪与网络有关的所有系统调用.

-e strace=signal 跟踪所有与系统信号有关的 系统调用

-e trace=ipc 跟踪所有与进程通讯有关的系统调用

-e abbrev=set 设定 strace输出的系统调用的结果集.-v 等与
abbrev=none.默认为abbrev=all.

-e raw=set 将指 定的系统调用的参数以十六进制显示.

-e signal=set 指定跟踪的系统信号.默认为all.如
signal=!SIGIO(或者signal=!io),表示不跟踪SIGIO信号.

-e read=set 输出从指定文件中读出 的数据.例如: -e read=3,5

-e write=set 输出写入到指定文件中的数据.

-o filename 将strace的输出写入文件filename

-p pid 跟踪指定的进程pid.

-s strsize 指定输出的字符串的最大长度.默认为32.文件名一直全部输出.

-u username 以username 的UID和GID执行被跟踪的命令

-c 统计每一系统调用的所执行的时间,次数和出错的次数等.

-d 输出strace关于标准错误的调试信息.

-f 跟踪由fork调用所产生的子进程.

-ff 如果提供-o
filename,则所有进程的跟踪结果输出到相应的filename.pid中,pid是各进程的进程号.

-F 尝试跟踪vfork调用.在-f时,vfork不被跟踪.

-h 输出简要的帮助信息.

-i 输出系统调用的入口指针.

-q 禁止输出关于脱离的消息.

-r 打印出相对时间关于,,每一个系统调用.

-t 在输出中的每一行前加上时间信息.

-tt 在输出中的每一行前加上时间信息,微秒级.

-ttt 微秒级输出,以秒了表示时间.

-T 显示每一调用所耗的时间.

-v
输出所有的系统调用.一些调用关于环境变量,状态,输入输出等调用由于使用频繁,默认不输出.

-V 输出strace的版本信息.

-x 以十六进制形式输出非标准字符串

-xx 所有字符串以十六进制形式输出.

示例
----

| ``strace cat /dev/null ``
| ``execve("/bin/cat", ["cat", "/dev/null"], [/* 15 vars */]) = 0``
| ``brk(0)                                  = 0x8b6b000``
| ``access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)``
| ``mmap2(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xb7741000``
| ``access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)``
| ``open("/etc/ld.so.cache", O_RDONLY)      = 3``
| ``fstat64(3, {st_mode=S_IFREG|0644, st_size=16285, ...}) = 0``
| ``mmap2(NULL, 16285, PROT_READ, MAP_PRIVATE, 3, 0) = 0xb773d000``
| ``close(3)                                = 0``
| ``access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)``
| ``open("/lib/i386-linux-gnu/libc.so.6", O_RDONLY) = 3``
| ``read(3, "\177ELF\1\1\1\0\0\0\0\0\0\0\0\0\3\0\3\0\1\0\0\0po\1\0004\0\0\0"..., 512) = 512``
| ``fstat64(3, {st_mode=S_IFREG|0755, st_size=1351816, ...}) = 0``
| ``mmap2(NULL, 1366328, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xb75ef000``
| ``mprotect(0xb7736000, 4096, PROT_NONE)   = 0``
| ``mmap2(0xb7737000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x147) =  0xb7737000``
| ``mmap2(0xb773a000, 10552, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xb773a000``
| ``close(3)                                = 0``
| ``mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xb75ee000``
| ``set_thread_area({entry_number:-1 -> 6, base_addr:0xb75ee8d0, limit:1048575, seg_32bit:1, contents:0,   read_exec_only:0, limit_in_pages:1, seg_not_present:0, useable:1}) = 0``
| ``mprotect(0xb7737000, 8192, PROT_READ)   = 0``
| ``mprotect(0x8054000, 4096, PROT_READ)    = 0``
| ``mprotect(0xb7760000, 4096, PROT_READ)   = 0``
| ``munmap(0xb773d000, 16285)               = 0``
| ``brk(0)                                  = 0x8b6b000``
| ``brk(0x8b8c000)                          = 0x8b8c000``
| ``open("/usr/lib/locale/locale-archive", O_RDONLY|O_LARGEFILE) = 3``
| ``fstat64(3, {st_mode=S_IFREG|0644, st_size=1747344, ...}) = 0``
| ``mmap2(NULL, 1747344, PROT_READ, MAP_PRIVATE, 3, 0) = 0xb7443000``
| ``close(3)                                = 0``
| ``fstat64(1, {st_mode=S_IFCHR|0600, st_rdev=makedev(136, 1), ...}) = 0``
| ``open("/dev/null", O_RDONLY|O_LARGEFILE) = 3``
| ``fstat64(3, {st_mode=S_IFCHR|0666, st_rdev=makedev(1, 3), ...}) = 0``
| ``fadvise64_64(3, 0, 0, POSIX_FADV_SEQUENTIAL) = 0``
| ``read(3, "", 32768)                      = 0``
| ``close(3)                                = 0``
| ``close(1)                                = 0``
| ``close(2)                                = 0``
| ``exit_group(0)                           = ? ``
| ``exit_group(0) = ?``

每一行都是一条系统调用，等号左边是系统调用的函数名及其参数，右边是该调用的返回值。
strace 显示这些调用的参数并返回符号形式的值。strace
从内核接收信息，而且不需要以任何特殊的方式来构建内核。 [2]_

--------------

``strace -o output.txt -T -tt -e trace=all -p 2375``

跟踪2375进程的所有系统调用（-e
trace=all），并统计系统调用的花费时间，以及开始时间（并以可视化的时分秒格式显示），最后将记录结果存在output.txt文件里面。 [3]_

--------------

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   ```Download``\ ````\ ``strace`` <http://sourceforge.net/projects/strace/files/strace/>`__

.. [2]
   `Linux
   strace命令 <http://www.cnblogs.com/ggjucheng/archive/2012/01/08/2316692.html>`__

.. [3]
   `Linux中的strace <http://www.dbabeta.com/2009/strace.html>`__
