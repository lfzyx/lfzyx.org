libvirt
========

libvirt 是支持不同的虚拟化技术的统一接口，libvirt 的主要目标是提供一个单一途径以管理多种不同虚拟化方案以及虚拟化主机，包括：KVM/QEMU，Xen，LXC，OpenVZ

libvirt 的一些主要功能如下 [#]_ ：

* VM management（虚拟机管理）：各种虚拟机生命周期的操作，如：启动、停止、暂停、保存、恢复和迁移等；多种不同类型设备的热插拔操作，包括磁盘、网络接口、内存、CPU等。

* Remote machine support（支持远程连接）：Libvirt 的所有功能都可以在运行着 libvirt 守护进程的机器上执行，包括远程机器。通过最简便且无需额外配置的 SSH 协议，远程连接可支持多种网络连接方式。

* Storage management（存储管理）：任何运行 libvirt 守护进程的主机都可以用于管理多种类型的存储：创建多种类型的文件镜像（qcow2，vmdk，raw，...），挂载 NFS 共享，枚举现有 LVM 卷组，创建新的 LVM 卷组和逻辑卷，对裸磁盘设备分区，挂载 iSCSI 共享，以及更多......

* Network interface management（网络接口管理）：任何运行 libvirt 守护进程的主机都可以用于管理物理的和逻辑的网络接口，枚举现有接口，配置（和创建）接口、桥接、VLAN、端口绑定。

* Virtual NAT and Route based networking（虚拟 NAT 和基于路由的网络）：任何运行 libvirt 守护进程的主机都可以管理和创建虚拟网络。Libvirt 虚拟网络使用防火墙规则实现一个路由器，为虚拟机提供到主机网络的透明访问。

程序和工具
-----------

在 Debian 中，这些程序使用了 libvirt 接口:

* virt-manager - 图形管理界面
* virt-viewer - 使用VNC连接虚拟机
* :doc:`virt-install` - 创建新的虚拟机
* :doc:`virsh` - 管理虚拟机
* virt-top - 类似于top的显示虚拟机资源的工具
* :doc:`virt-clone` 克隆虚拟机

.. rubric:: 参考文献

.. [#] `Libvirt (简体中文) - ArchWiki <https://wiki.archlinux.org/index.php/Libvirt_(简体中文)>`_