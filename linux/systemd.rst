.. index:: Systemd

Systemd - 系统启动管理
======================

linux 在开机加载内核后，便由内核加载 systemd init 系统，再由 systemd
init 系统加载用户空间和启动进程。systemd
控制系统服务间的依赖关系，并依此实现系统初始化时服务的并行启动。

设计
----

单元
^^^^

systemd 的核心是一个叫单元 unit 的概念，systemd
使用声明性语句在配置文件中为每个守护进程记录初始化指令，配置文件被称之为
Unit file 。 Unit 一共分成12种:

- Service unit：系统服务
- Target unit：多个 Unit 构成的一个组
- Device Unit：硬件设备
- Mount Unit：文件系统的挂载点
- Automount Unit：自动挂载点
- Path Unit：文件或路径
- Scope Unit：不是由 Systemd 启动的外部进程
- Slice Unit：进程组
- Snapshot Unit：Systemd 快照，可以切回某个快照
- Socket Unit：进程间通信的socket
- Swap Unit：swap 文件
- Timer Unit：定时器

一个sshd.service 是一个 Unit，一个 multi-user.target 集合也是一个
Unit。Systemd
的其中一个目标就是简化这些单元之间的相互作用，因此如果你有程序需要在某个挂载点被创建或某个设备被接入后开始运行，Systemd
可以让这一切正常运作起来变得相当容易。

核心组件和库
^^^^^^^^^^^^

-  systemd 是一个系统和服务管理程序.
-  systemctl 是与 Systemd 交互的控制程序.
-  systemd-analyze
   用于确定系统启动性能统计信息，从系统和服务管理器中了解其他状态和跟踪信息

systemd 用 linux 内核的 cgroups 子系统代替 PIDs
来追踪进程，以此即使是两次fork之后生成的守护进程也不会脱离systemd的控制。

辅助部件
^^^^^^^^

-  systemd-journald
   负责事件记录的守护进程，二进制文件作为其日志文件，系统管理员可以选择使用
   systemd-journald，syslog-ng或rsyslog 记录系统事件。
-  systemd-logind
   管理用户登陆的守护进程，它是一个集成的登录管理器，替代不再维护的ConsoleKit
-  networkd 处理网络接口配置的守护进程
-  timedated 控制时间设置的守护进程
-  udevd 驱动管理器
-  systemd-boot 启动管理器

使用
----

systemctl
^^^^^^^^^^

-  systemctl status 显示系统状态
-  systemctl list-units 输出已加载的单元
-  systemctl list-units --all 输出已加载的单元（包含未激活）
-  systemctl list-units --failed 输出运行失败的单元
-  systemctl list-unit-files 查看所有已安装单元，自动启动 enabled
   的单元显示为绿色，被禁用自动启动 disabled 的显示为红色。标记为 static
   的单元不能直接启用，它们是其他单元所依赖的对象，masked
   是被禁用的单元。所有可用的单元文件存放在 /lib/systemd/system 和
   /etc/systemd/system/
   目录（后者优先级更高）。在单元配置文件中，您可以看到各种选项，包括要被运行的二进制文件（“ExecStart”那一行），相冲突的其他单元（即不能同时进入运行的单元），以及需要在本单元执行前进入运行的单元（“After”那一行）。一些单元有附加的依赖选项，例如“Requires”（必要的依赖）和“Wants”（可选的依赖）。
-  systemctl start 立即激活单元
-  systemctl stop 立即停止单元
-  systemctl restart 重启单元
-  systemctl reload 重新加载配置
-  systemctl status 输出单元状态
-  systemctl is-enabled 检查单元是否配置为自动启动
-  systemctl enable
   开机自动启动单元,这会为该单元创建一个符号链接，并将其放置在当前启动目标的
   .wants 目录下，这些 .wants 目录在/etc/systemd/system 文件夹中。
-  systemctl disable 取消开机自动启动单元
-  systemctl mask 禁用一个单元
-  systemctl unmask 取消禁用一个单元
-  systemctl list-dependencies 列出一个 Unit 的所有依赖

使用 systemctl 控制单元时，通常需要使用单元文件的全名，包括扩展名（例如
sshd.service ）。但是有些单元可以在 systemctl 中使用简写方式。

journalctl
^^^^^^^^^^^^

-  journalctl -b 输出本次启动的日志
-  journalctl -b --since=”2016-10-24 16:38”
   2016年10月24日16:38以来的日志

systemd-analyze
^^^^^^^^^^^^^^^^

-  systemd-analyze blame 查看每个服务的启动耗时
-  systemd-analyze critical-chain 显示瀑布状的启动过程流
