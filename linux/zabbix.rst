简介
----

Zabbix 是由 Alexei Vladishev 开发的一种网络监视、管理系统，基于
Server-Client
架构。可用于监视各种网络服务、服务器和网络机器等状态。 [1]_

使用各种 Database-end 如 MySQL, PostgreSQL, SQLite, Oracle 或 IBM DB2
储存资料。Server 端基于 C语言、Web 管理端 frontend 则是基于 PHP
所制作的。Zabbix 提供多种方式监视：

-  可以只使用 Simple Check 不需要安装 Client 端，基于 SMTP 或 HTTP
   等协议验证可用性。
-  在需要监控的机器上安装 Zabbix agent 来监控 CPU load, network
   utilization, disk space 等等
-  作为安装 agent 的替代，Zabbix 支持通过 SNMP, TCP and ICMP 协议，用
   IPMI、SSH、telnet 对目标进行监控

架构
----

Zabbix包含如下组件

Server
~~~~~~

Zabbix server
是Zabbix软件的中心进程，用来获取agent存活状况及监控数据和统计.
所有的配置、统计、操作数据均通过Server进行存取.

Server
执行polling和trapping来采集数据，评估是否触发触发器，发送报警给用户.

Server
是所有配置、统计和操作数据的中心存储仓库，也是在所有的监控系统中扮演故障发生时通知管理员的角色.

基础 server依据功能不同划分为三个部分，分别为:Zabbix
server、web前端及数据库.

由于Zabbix的所有的配置信息保存在数据库中，server和web前端可以直接进行操作。比如，通过web前端(或者API)创建一个新的监控项时，它将创建的数据插入数据库。一分钟左右Zabbix
server会查询监控项数据表，并将查询的监控项
列表保存在自己的缓存(cache)中。这也是为什么通过Zabbix前端进行的变更将在两分钟左右生效.

Database storage
~~~~~~~~~~~~~~~~

所有信息存放在数据库

Web interface
~~~~~~~~~~~~~

web接口. 该接口作为Zabbix
Server的一部分，通常和server运行在同一台主机上.

Proxy
~~~~~

Zabbix代理(proxy)通常用于替代server完成对多个监控设备收集监控信息并将数据发送给Zabbix
server. 所有的收集数据会先存储在代理本地缓存中然后传送给Zabbix server.

代理是可选的,不过使用它可以有效的降低在分布式环境中单一的Zabbix
server负载. 通过代理去收集收集，server可以有效降低CPU和磁盘I/O消耗.

Zabbix代理可以出色的完成远程区域、分支机构、没有本地管理员的网络的集中监控.

Zabbix代理使用独立的数据库.

Agents
~~~~~~

Zabbix agent 部署在目标监控机上并监控本地资源和应用.

Zabbix agent 聚合本地的运行信息并将数据发送给Zabbix server以进一步处理.
一旦出现异常(如硬盘空间满的情况或者服务进行宕掉), Zabbix
server将会发送告警给管理员报告本次异常.

Zabbix agent利用本地系统调用完成统计信息收集，因此它非常的高效.

Zabbix agent提供被动和主动检查方式.在 被动检查
模式中agent应答数据请求，Zabbix server或者proxy询问agent数据,如CPU load,
然后Zabbix agent回送结果给server.主动检查 处理过程将相对复杂.
agent必须首先进行一次请求Zabbix
server索取监控项列表，然后发送对应的值给server.选择是被动还是主动检查，需要在
监控项类型 中选择’Zabbix agent’或者’Zabbix agent (active)’.

Data flow
~~~~~~~~~

了解 zabbix
内部整个数据流是非常重要的。为了创建一个收集数据的项目，必须首先创建一个主机，在这之前必须要创建触发器，必须有一个触发器来创建一个动作。因此，如果您希望收到服务器CPU加载过高的报警，需要创建一个主机条目，并且跟随一个CPU警报条目，这样在CPU过高的时候，触发器会被触发，并发送电子邮件。看起来步骤很多，但如果使用模版，就很方便了。

释义
----

-  host 一个你想监控的网络设备(需要知道IP/DNS)

-  host group 一个逻辑的主机群组，它包含主机和模板。
   主机和模板在同一个主机群组内的话模板不能被互相linked。
   主机组通常用于给不同的用户组创建访问权限

-  item 你想从主机中收集到的数据

-  trigger
   一个逻辑表达式，用来表达从监控项获取的数据达到了预设的问题阀值，当接收到的监控值达到了预设的阀值，则触发器状态由’OK’变更为’Problem’，当收到的监控值低于阀值，则状态保持/变更为’OK’.

-  event 一个事情发生，如触发器状态变更或一个自动发现/agent自动注册等

-  action
   当一个事件发生时预设的处理过程，一个动作(action)包括操作(operations，如发送告警)和条件(当指定的操作完成)

-  escalation
   在动作中一个自定的操作执行过程，一个发送告警/执行远程命令的队列

-  media 发送告警的渠道

-  notification 通过媒介(media)渠道发送事件的消息

-  remote command
   当监控主机达到某些条件(condition)后预设的自动执行的命令

-  template
   一组包含监控项、触发器、绘图、面板(screen)、应用、低级别自动发现规则等并且能被其他主机应用的实体。模板能够提升主机部署监控任务的速度，同时也非常容易对监控任务做批量(mass)更新。

-  application 监控项逻辑组

-  web scenario 对一个web站点可用性进行检查的一个或多个http请求

-  Zabbix API允许通过JSON
   RPC协议去创建、更新、获得Zabbix对象(如主机、监控项、绘图等等)以及完成自定义任务

安装
----

Zabbix server
~~~~~~~~~~~~~

| ``wget ``\ ```http://repo.zabbix.com/zabbix/3.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.0-1+trusty_all.deb`` <http://repo.zabbix.com/zabbix/3.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_3.0-1+trusty_all.deb>`__
| ``dpkg -i zabbix-release_3.0-1+trusty_all.deb``
| ``apt-get update``

-  Zabbix server with web frontend , mysql database.

``apt-get install zabbix-server-mysql zabbix-frontend-php``

-  初始化数据库

| ``mysql -uroot -p``\ 
| ``create database zabbix character set utf8 collate utf8_bin;``
| ``grant all privileges on zabbix.* to zabbix@localhost identified by '``\ \ ``';``
| ``exit;``
| ``cd /usr/share/doc/zabbix-server-mysql``
| ``zcat create.sql.gz | mysql -uroot zabbix``

-  zabbix_server.conf

| ``vi /etc/zabbix/zabbix_server.conf``
| ``DBHost=localhost``
| ``DBName=zabbix``
| ``DBUser=zabbix``
| ``DBPassword=zabbix``

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `Zabbix - Wikipedia, the free
   encyclopedia <https://en.wikipedia.org/wiki/Zabbix>`__
