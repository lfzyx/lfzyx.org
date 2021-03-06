sysctl用语配置内核参数，支持的参数都在 /proc/sys 中列出

语法
----

-a, --all 显示当前所有的内核参数值

-p 从 /etc/sysctl.conf 加载配置

配置
----

*sysctl.conf 中的参数配置是与 /proc/sys 中的文件相关联的，将文件路径中 /
转换为 . 即可*

-  net.ipv4.tcp_syncookies = 1

表示开启 TCP/IP SYN cookies
，当出现SYN等待队列溢出时，启用cookies来处理，可防范少量SYN攻击

-  fs.file-max

Linux系统最多允许同时打开(即包含所有用户打开文件数总和)的文件数，是Linux系统级硬限制，所有用户级的打开文件数限制都不应超过这个数值。通常这个系统级硬限制是Linux系统在启动时根据系统硬件资源状况计算出来的最佳的最大同时打开文件数限制，如果没有特殊需要，不应该修改此限制，除非想为用户级打开文件数限制设置超过此限制的值。
