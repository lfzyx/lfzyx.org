Crontab - 周期性任务
=====================

cron是类unix系统中的周期性任务工具 [#]_ ,使系统维护和管理能够自动化。cron这个名称来自于古希腊语中的“时间”，χρόνος
柯罗诺斯


配置
----

cron的配置文件为 crontab ，是cron table的简写，分为系统 crontab 和用户
crontab

系统crontab
^^^^^^^^^^^

由系统管理员制定的系统维护以及其他任务的crontab文件，与用户任务配置文件格式略有不同，可以通过USERNAME字段指定由哪个用户身份来执行任务。

::

 cat /etc/crontab

 SHELL=/bin/sh  指定要使用的shell
 PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin  指定了系统执行命令的路径
 MAILTO=USERNAME  通过电子邮件发送任务执行信息给用户
 HOME=/  执行任务时使用的主目录
 * * * * *  USERNAME 命令

用户crontab
^^^^^^^^^^^

用户crontab文件存放在/var/spool/cron/\ **[crontabs]**\ 目录下，以所属用户的登录名来命名，cron根据文件名来决定执行任务时应该使用的用户身份。

::

 cat /var/spool/cron/[crontabs] USERNAME

 * * * * * 命令

crontab格式
~~~~~~~~~~~

::

  * * * * *   [USERNAME] 命令
  ┬ ┬ ┬ ┬ ┬
  │ │ │ │ │
  │ │ │ │ │
  │ │ │ │ └────── 星期 (0 - 6) (0 至 6 是周日至周六)
  │ │ │ └────── 月份 (1 - 12)
  │ │ └────── 日期 (1 - 31)
  │ └────── 小时 (0 - 23)
  └────── 分钟 (0 - 59)

-  cron每分钟读取一次crontab文件，按照crontab文件中给出的时间执行指定的任务。
-  crontab文件的每一行代表一个任务，均遵守特定的格式，由空格或tab分隔。
-  前5个时间字段可以使用特殊字符

 #. 星号(*) 匹配任何可能的值
 #. 划线(-) 隔开两个值，设定范围
 #. 逗号(,) 隔开整数或范围，匹配所有列出的值
 #. 斜线(/) 设置间隔

-  星期和日期有潜在的二义性，如果同时指定了星期和日期，那么只要有一个条件满足就会执行。
-  ***USERNAME***\ 字段只出现在系统crontab文件/etc/crontab和/etc/cron.d目录下的文件，用来指定由哪个用户身份来执行任务。这个字段不会也没有必要出现在用户crontab文件中，因为用户crontab文件名暗含了UID
-  命令可以是系统命令也可以是编写的脚本

语法
----

::

 crontab [OPTION] FILE
 crontab [OPTION]

将FILE做为crontab的任务列表文件载入crontab，替换之前的所有版本

-u user 用来设定某个用户的crontab服务

-e 编辑某个用户crontab文件内容。如果不指定用户，则表示编辑当前用户的crontab文件

-l 显示某个用户crontab文件内容，如果不指定用户，则表示显示当前用户的crontab文件内容

-r 删除某个用户crontab文件，如果不指定用户，则默认删除当前用户的crontab文件

-i 在删除用户crontab文件时给确认提示

权限
----

用/etc/cron.deny 和 /etc/cron.allow
指定哪些用户可以提交crontab文件。如果存在cron.allow文件，那么它将包含可以提交crontan的所有用户名单，名单每行一个用户，
任何没被列出的用户都不能使用crontab。如果没有cron.allow文件，那么将检查是否有cron.deny文件，除了名单中列出的用户，
所有其他用户都允许使用crontab。如果cron.allow和cron.deny文件都不存在，那么就只有root用户才能使用crontab（debian允许所有用户使用crontab）。

示例
----

::

 crontab -u root -e 编辑root用户的crontab文件

::

 * * * * * command 每1分钟执行一次 [#]_
 3,15 * * * * command 每小时的第3和第15分钟执行
 3,15 8-11 * * * command 在8点到11点的第3和第15分钟执行
 3,15 8-11 */2 * * command 每隔两天的8点到11点的第3和第15分钟执行
 3,15 8-11 * * 1 command 每个星期一的8点到11点的第3和第15分钟执行
 30 21 * * * command 每晚的21:30执行
 45 4 1,10,22 * * command 每月1、10、22日的4:45执行
 10 1 * * 6,0 command 每周六、周日的1:10执行
 0,30 18-23 * * * command 每天18点至23点之间每隔30分钟执行
 0 23 * * 6 command 每个星期六的23点执行
 * */1 * * * command 每一小时执行
 * 23-7/1 * * * commadn 晚上23点到早上7点之间，每隔一小时执行

保存文件时，
cron会对其进行检查，如果其中的某个字段出现了超出允许范围的值，cron会发出警告

.. rubric:: 参考文献

.. [#] `Cron - Wikipedia <http://en.wikipedia.org/wiki/Cron>`_
.. [#] `每天一个linux命令（50）：crontab命令 <http://www.cnblogs.com/peida/archive/2013/01/08/2850483.html>`_
