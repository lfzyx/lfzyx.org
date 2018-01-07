.. index:: Audit

Audit - 安全审计
================

Audit 是 linux 上的审计工具, 用于记录 linux 上的动作. 通过日志文件, 我们可以跟踪安全事件, 查明滥用和未授权的活动，包括：

* 跟踪文件访问
* 监控系统调用
* 记录命令执行
* 记录安全事件
* 事件搜索
* 报告总结
* 监控网络访问

安装
------

:literal:`apt install auditd`

:literal:`systemctl start auditd`

Audit 系统由两部分组成:

#. audit 核心部件监听系统调用，记录事件，发送消息至 audit daemon
#. audit daemon 收集消息，输出条目至日志文件


配置
-------

配置文件为 /etc/audit/auditd.conf [#]_

- num_logs 日志轮循个数
- max_log_file 单个日志文件大小
- max_log_file_action 到达日志大小限制触发的行为

规则文件为 /etc/audit/audit.rules


auditctl
-------------------------
auditctl 控制审计系统的行为

-l  查看审计规则
-w  添加文件路径
-p  指定触发审计的权限，r=read, w=write, x=execute, a=attribute
-k  添加规则时设置关键字

**示例**

:literal:`auditctl -w /etc/passwd -p rwxa`


aureport
-------------------------
aureport 通过日志生成一份总结性的报告

**示例**

::

 Summary Report
 ======================
 Range of time in logs: 12/22/2017 16:24:40.330 - 12/25/2017 15:37:57.867
 Selected time for report: 12/22/2017 16:24:40 - 12/25/2017 15:37:57.867
 Number of changes in configuration: 11
 Number of changes to accounts, groups, or roles: 0
 Number of logins: 0
 Number of failed logins: 0
 Number of authentications: 8
 Number of failed authentications: 0
 Number of users: 5
 Number of terminals: 12
 Number of host names: 6
 ...


ausearch
----------------------
ausearch 查找审计事件

-f  基于文件路径查找事件
-m  基于类型查找事件
-a  基于 audit message ID 查找时间
-k  基于关键字查找事件

**示例**

:literal:`ausearch -i -f /etc/passwd`

::

 type=PROCTITLE msg=audit(12/22/2017 17:01:10.706:180) : proctitle=rm
 type=PATH msg=audit(12/22/2017 17:01:10.706:180) : item=1 name=config.py inode=111706342 dev=fe:05 mode=file,644 ouid=qc ogid=qc rdev=00:00 nametype=DELETE
 type=PATH msg=audit(12/22/2017 17:01:10.706:180) : item=0 name=/opt inode=111706226 dev=fe:05 mode=dir,755 ouid=qc ogid=qc rdev=00:00 nametype=PARENT
 type=CWD msg=audit(12/22/2017 17:01:10.706:180) :  cwd=/opt/config.py
 type=SYSCALL msg=audit(12/22/2017 17:01:10.706:180) : arch=x86_64 syscall=unlinkat success=yes exit=0 a0=0xffffffffffffff9c a1=0x19bc0c0 a2=0x0 a3=0x7ffd05556870 items=2 ppid=67063 pid=68083 auid=lfzyx uid=root gid=root euid=root suid=root fsuid=root egid=root sgid=root fsgid=root tty=pts2 ses=20178 comm=rm exe=/bin/rm key=qc



* type : audit message 类型
* name : 审计对象
* arch : CPU 架构
* syscall : 系统调用类型
* success : 系统调用是否成功
* ppid : 父进程 ID
* pid : 进程 ID
* auid : 原始 UID
* uid 和 gid : 访问文件的用户ID和用户组ID
* cwd : 当前路径
* comm : 触发这条 audit message 的命令
* exe : 命令的路径
* key : 规则定义的关键字 [#]_

.. rubric:: 参考文献

.. [#] `Linux 用户空间审计工具 audit <https://www.ibm.com/developerworks/cn/linux/l-lo-use-space-audit-tool/index.html>`_
.. [#] `UNDERSTANDING AUDIT LOG FILES <https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/security_guide/sec-understanding_audit_log_files>`_