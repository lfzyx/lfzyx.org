.. index:: KVM

KVM - 虚拟化解决方案
=====================

简介
-----

KVM（Kernel-based Virtual Machine 基于内核的虚拟机) 是具备Intel VT或
AMD-V功能的 x86
平台全虚拟化解决方案，它包含一个为处理器提供底层虚拟化可加载的核心模块kvm.ko
(kvm-intel.ko或kvm-AMD.ko)。仅有KVM模块是远远不够的，
KVM内核模块本身只能提供CPU和内存的虚拟化，用户无法直接控制内核模块去作事情，KVM还需要一个经过修改的 :doc:`qemu`
软件作为虚拟机上层控制，才能构成一个完成的虚拟化技术。 :doc:`qemu`
也是一个虚拟化软件。它的特点是可虚拟不同的CPU。比如说在x86的CPU上可虚拟一个Power的CPU，并可利用它编译出可运行在Power上的程序。

安装
-----

:literal:`apt-get install qemu-kvm libvirt-bin`  [#]_

KVM 使用了 QEMU 的基于 x86 的部分，并稍加改造，形成可控制 KVM
内核模块的用户空间工具 QEMU-KVM。 [#]_ QEMU-KVM
通过ioctl调用/dev/kvm接口，将有关CPU指令的部分交由内核模块来做。KVM
实现了cpu和内存的虚拟化，qemu模拟IO设备。

QEMU-KVM 工具效率不高，不易于使用，RedHat 为 KVM
开发了更多的辅助工具，比如 :doc:`libvirt`

:doc:`libvirt` 是一套提供了多种语言接口的 API，为各种虚拟化工具提供一套方便、可靠的编程接口，不仅支持 KVM，而且支持 Xen
等其他虚拟机，而且一些常用的虚拟机管理工具（如 :doc:`virsh` 、 :doc:`virt-install` 、virt-manager等）和云计算框架平台（如OpenStack、OpenNebula、Eucalyptus等）都在底层使用libvirt的应用程序接口。

为了方便管理虚拟机，应该把当前的操作用户加入 kvm 与 libvirt
组。这样就可以使用 virsh 命令了。

使用
-----

设置桥接网络
^^^^^^^^^^^^

默认情况下，:doc:`qemu` 会给 客户机提供 NAT
网络，这种情况下主机和客户机访问不到对方。为了给客户机提供完整的网络访问权限，应该创建网桥。 [#]_

:literal:`apt-get install bridge-utils`

修改 /etc/network/interfaces文件，让网桥br0代理eth0。配置完成后，在客户机中使用网桥接口。

::

 # The loopback network interface
 auto lo
 iface lo inet loopback
 # The primary network interface
 auto br0
 iface br0 inet static 
   address 10.10.10.70 
   netmask 255.255.255.0  
   gateway 10.10.10.10  
   bridge_ports eth0  
   bridge_stp off  
   bridge_fd 0   
   bridge_maxwait 0

创建新的主机
^^^^^^^^^^^^

virtinst 提供命令行 :doc:`virt-install` 来创建虚拟机。

:literal:`apt-get install virtinst`

::

 virt-install
   --connect qemu:///system \
   --virt-type kvm \
   --name demo \
   --memory 500 \
   --disk size=20 \
   --cdrom /var/lib/libvirt/boot/debian-8.5.0-amd64-netinst.iso \
   --os-variant debianwheezy \
   --network bridge=br0 \
   --noautoconsole \
   --graphics vnc,password=XYZ12345,listen=0.0.0.0


创建成功后使用 :doc:`virsh` -c qemu:///system list --all
查看虚拟机状态，running 状态表示创建启动成功，使用 VNC
客户端连接服务器即可完成安装。

如果需要预置自动安装请参考 preseed [#]_ .

启动与停止
^^^^^^^^^^^^

在登录常规用户的情况下 :doc:`libvirt` 默认情况会使用
qemu:///session 域，添加 -c qemu:///system 选项可使用 qemu:///system
域。

查看客户机列表

:literal:`virsh -c qemu:///system list --all`

停止一个客户机

:literal:`virsh -c qemu:///system shutdown demo`

如果停止失败，可以强制停止

:literal:`virsh -c qemu:///system destroy demo`

启动客户机

:literal:`virsh -c qemu:///system start demo`

设置虚拟机开机启动

:literal:`virsh autostart alice`


.. rubric:: 参考文献

.. [#] `KVM-Debian Wiki <https://wiki.debian.org/KVM#Installation>`_
.. [#] `虚拟化之QEMU与KVM <http://blog.chinaunix.net/uid-23769728-id-3256677.html>`_
.. [#] `BridgeNetworkConnections <https://wiki.debian.org/BridgeNetworkConnections>`_
.. [#] `preseed <https://www.debian.org/releases/stable/amd64/apb.html.zh-cn>`_

