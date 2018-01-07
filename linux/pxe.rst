PXE - 预启动执行环境
=====================

PXE(Preboot eXecution
Environment)预启动执行环境,提供了一种使用网络接口（Network
Interface）启动计算机的机制。这种机制让计算机的启动可以不依赖本地数据存储设备（如硬盘）或本地已安装的操作系统。 [#]_

安装
----

dnsmasq是一个轻量级网络基础架构服务器，它可以通过内部集成的DNS、DHCP和TFTP服务器提供网络启动服务。

::

 apt-get install dnsmasq
 mkdir /var/ftpd
 cd /var/ftpd
 wget http://ftp.nl.debian.org/debian/dists/wheezy/main/installer-amd64/current/images/netboot/mini.iso
 wget http://ftp.nl.debian.org/debian/dists/wheezy/main/installer-amd64/current/images/netboot/pxelinux.0
 wget http://ftp.nl.debian.org/debian/dists/wheezy/main/installer-amd64/current/images/netboot/netboot.tar.gz
 tar xfz netboot.tar.gz

配置
----

::

 vi /etc/dnsmasq.conf

 interface=eth0 *服务器监听的网络接口*
 dhcp-range=10.10.10.80,10.10.10.89,255.255.255.0,12h *用你自己的网络掩码定义的网络IP地址范围*
 dhcp-boot=pxelinux.0 *定义pxe文件*
 enable-tftp *启用内建TFTP服务器*
 tftp-root=/var/ftpd *使用/var/ftpd作为Debian网络启动文件的存放位置*

.. rubric:: 参考文献

.. [#] `Preboot Execution Environment - Wikipedia <http://en.wikipedia.org/wiki/Preboot_Execution_Environment>`_
