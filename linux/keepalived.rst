.. index:: Keepalived

Keepalived - 高可用解决方案
===========================

Keepalived是一个基于VRRP [#]_ 协议来实现的服务高可用方案，主要有三个模块，分别是 core、check 和 VRRP。

core 模块为 keepalived 的核心，负责主进程的启动、维护以及全局配置文件的加载和解析，check 负责健康检查，包括常见的各种检查方式，VRRP 模块是来实现 VRRP 协议的。

基于这些模块，keepalived 采用了多进程的设计模式，每个进程对应一个模块。

安装
------

:literal:`apt-get install keepalived`

配置
------

keepalived只有一个配置文件keepalived.conf，里面主要包括以下几个配置区域，分别是global_defs、static_ipaddress、static_routes、vrrp_script、vrrp_instance和virtual_server。 [#]_

global_defs
^^^^^^^^^^^^^^

global_defs用于设置 keepalived 的通知机制和标识

::

 global_defs
 {
    notification_email
    {
        lfzyx@lfzyx.org
    }
    notification_email_from lfzyx@lfzyx.org
    smtp_server mail.lfzyx.org
    smtp_connect_timeout 30
    router_id hostname
 }

- notification_email 指定 keepalived 发生事件时，用邮件通知的email地址

- router_id 设置机器的标识

vrrp_script
^^^^^^^^^^^^^^

告诉 keepalived 在什么情况下降低优先级，可以有多个 vrrp_script

::

 vrrp_script chk_nginx
 {
    script "pidof nginx"
    interval 1
    weight -5
    fall 1
    rise 2
 }

- script : 自己写的检测脚本。也可以是一行命令如 pidof nginx
- interval 2 : 每2s检测一次
- weight -5 : 检测失败（脚本返回非0）则优先级 -5
- fall 2 : 检测连续 2 次失败才算确定是真失败。会用weight减少优先级（1-255之间）
- rise 2 : 检测 2 次成功就算成功。但不修改优先级

vrrp_instance
^^^^^^^^^^^^^^^^

vrrp_instance用来定义对外提供服务的VIP区域及其相关属性 [#]_

::

 vrrp_instance MASTER
  {
    state MASTER
    interface eth0
    mcast_src_ip 10.10.10.71
    virtual_router_id 73
    priority 200
    advert_int 1
    authentication
    {
        auth_type PASS
        auth_pass passwd
    }
    virtual_ipaddress
    {
        10.10.10.73
    }
    track_script
    {
       chk_nginx
    }
 }

- state : 指定instance(Initial)的初始状态，但启动后会发生竞选，高优先级的会抢占为MASTER
- interface : 实例绑定的网卡，因为在配置虚拟IP的时候必须是在已有的网卡上添加的
- mcast_src_ip :发送多播数据包时的源IP地址，如果没有设置那么就用绑定的网卡的 primary IP
- virtual_router_id : 这里设置VRID，这里非常重要，相同的VRID为一个组，他将决定多播的MAC地址
- priority : 设置本节点的优先级，优先级高的为master
- advert_int : 检查间隔，默认为1秒。这就是VRRP的定时器，MASTER每隔这样一个时间间隔，就会发送一个advertisement报文以通知组内其他路由器自己工作正常
- authentication : 定义认证方式和密码，主从必须一样
- virtual_ipaddress : 这里设置的就是VIP，也就是虚拟IP地址，他随着state的变化而增加删除，当state为master的时候就添加，当state为backup的时候删除
- track_script : 引用VRRP脚本，即在 vrrp_script 部分指定的名字。定期运行它们来改变优先级，并最终引发主备切换。

.. rubric:: 参考文献

.. [#] VRRP全称 Virtual Router Redundancy Protocol，即 虚拟路由冗余协议。可以认为它是实现路由器高可用的容错协议，即将N台提供相同功能的路由器组成一个路由器组(Router Group)，这个组里面有一个master和多个backup，但在外界看来就像一台一样，构成虚拟路由器，拥有一个虚拟IP（vip，也就是路由器所在局域网内其他机器的默认路由），占有这个IP的master实际负责ARP相应和转发IP数据包，组中的其它路由器作为备份的角色处于待命状态。master会发组播消息，当backup在超时时间内收不到vrrp包时就认为master宕掉了，这时就需要根据VRRP的优先级来选举一个backup当master，保证路由器的高可用。
.. [#] `linux/keepalived at master · chenzhiwei/linux <https://github.com/chenzhiwei/linux/tree/master/keepalived>`_
.. [#] `Nginx+Keepalived实现站点高可用 | Sean's Notes <http://seanlook.com/2015/05/18/nginx-keepalived-ha/>`_
