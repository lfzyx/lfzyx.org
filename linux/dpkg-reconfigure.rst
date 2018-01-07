Dpkg-reconfigure
================

重新配制一个已经安装的软件包.

语法
----

``dpkg-reconfigure ``\ ***``[OPTION]``***

-a, --all 重配置所有软件包

-u, --unseen-only 仅显示未提过的问题

--default-priority 使用默认优先级，而非“低”级

--force 强迫重配置受损软件包

--no-reload 不要轻易的重装模板(使用时请慎重考虑)

-f, --frontend 指定 debconf 前端界面

-p, --priority 指定要显示的问题的最优先级

--terse 开启简要模式

示例
----

| ``# dpkg-reconfigure tzdata ``\ *``更改系统时区``*
| ``# dpkg-reconfigure locales ``\ *``更改系统语言``*
| ``# dpkg --add-architecture i386 ``\ *``支持多架构``*
