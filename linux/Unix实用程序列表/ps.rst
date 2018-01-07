ps aux 查看所有进程
ps axwef 以树状列出进程及子进程
ps lax 显示父进程PPID和谦让值NI
ps -fp [pid pid] 查看多个进程信息
pstree 以树状格式显示当前运行的所有进程及其相关的子进程
pgrep -f 通过进程名过滤进程
ps -LF -u root 输出用户的进程和线程。选项“L”（列出线程），选项“-F”（完整格式化）
ps aux --sort=-%mem 按内存使用率排序