Virt-install
=============

* 可选参数

--connect URI 设置要连接的域

* 通用选项

--name NAME  虚拟机的名称

--memory MEMORY  虚拟机的内存

--vcpus Number 分配CPU核心数

--cpu CPU cpu模式

* 安装方法选项

--cdrom CDROM 安装使用的iso镜像

--:doc:`pxe` 使用网络启动

--import 通过存在的磁盘镜像创建虚拟机

--livecd 将 CD-ROM 作为 Live CD

--os-variant DISTRO_VARIANT 设置虚拟机系统版本

* 存储配置

--disk DISK 定义虚拟机使用的媒介

* 网络配置

--network NETWORK

* 图形配置

--graphics GRAPHICS

* 外设选项

--controller CONTROLLER

* 虚拟化平台选项

--hvm  全虚拟化

--paravirt 半虚拟化

--virt-type HV_TYPE   虚拟化类型 (kvm, qemu, xen, ...)

* 其他选项

--autostart  虚拟机开机自启动

--noautoconsole   不会尝试自动连接虚拟机控制台

--noreboot    安装完成后不会自启动

--debug    输出debug消息

示例
-----

安装 debian8.5， 创建10GB的qcow2文件，从iso镜像安装， 启动VNC控制端。

::

   virt-install \
     --connect qemu:///system \
     --virt-type kvm \
     --name demo \
     --memory 500 \
     --disk size=20 \
     --cdrom /var/lib/libvirt/boot/debian-8.5.0-amd64-netinst.iso \
     --os-variant debianwheezy \
     --network bridge=br0 \
     --noautoconsole \
     --graphics vnc,password=XYZ12345,listen=0.0.0.0
