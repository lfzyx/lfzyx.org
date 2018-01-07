NFS - 分布式文件系统
====================

网络文件系统（英语：Network File
System，缩写为NFS）是一种分布式文件系统协议，其功能旨在允许客户端主机可以像访问本地存储一样通过网络访问服务器端文件。

NFS在文件传送或信息传送过程中依赖于RPC协议。RPC，远程过程调用 (Remote
Procedure Call)
是能使客户端执行其他系统中程序的一种机制。NFS本身是没有提供信息传输的协议和功能的，但NFS却能让我们通过网络进行资料的分享，这是因为NFS使用了一些其它的传输协议。而这些传输协议用到这个RPC功能的。可以说NFS本身就是使用RPC的一个程序。或者说NFS也是一个RPC
SERVER。所以只要用到NFS的地方都要启动RPC服务，不论是NFS SERVER或者NFS
CLIENT。这样SERVER和CLIENT才能通过RPC来实现PROGRAM
PORT的对应。可以这么理解RPC和NFS的关系：NFS是一个文件系统，而RPC是负责负责信息的传输。

安装
----

-  服务端

``apt-get install nfs-kernel-server nfs-common``

-  客户端

``apt-get install nfs-common``

使用
----

服务端配置
~~~~~~~~~~

配置文件: /etc/exports [#]_

格式: [共享的目录] [客户端(参数1,参数2)]

客户端是指网络中可以访问这个NFS输出目录的计算机，客户端常用的指定方式：

-  指定ip地址的主机：192.168.0.200
-  指定子网中的所有主机：192.168.0.0/24 192.168.0.0/255.255.255.0
-  所有主机：\*

参数选项用来设置输出目录的访问权限、用户映射等。

访问权限选项:

-  设置输出目录只读：ro
-  设置输出目录读写：rw

用户映射选项:

-  all_squash：不管使用NFS的用户是谁，他的身份都会被限定成为一个指定的普通用户身份；
-  no_all_squash：与all_squash取反（默认设置）；
-  root_squash：root用户对共享目录的权限不高，只有普通用户的权限，即限制了root（默认设置）；
-  no_root_squash：root用户对共享的目录拥有至高的权限控制，就像是对本机的目录操作一样。
-  anonuid/anongid：要和root_squash 以及
   all_squash一同使用，用于指定使用NFS的用户限定后的uid和gid

其它选项:

-  secure：限制客户端只能从小于1024的tcp/ip端口连接nfs服务器（默认设置）；
-  insecure：允许客户端从大于1024的tcp/ip端口连接服务器；
-  sync：将数据同步写入内存缓冲区与磁盘中，效率低，但可以保证数据的一致性；
-  async：将数据先保存在内存缓冲区中，必要时才写入磁盘(建议设置)；
-  wdelay：检查是否有相关的写操作，如果有则将这些写操作一起执行，这样可以提高效率（默认设置）；
-  no_wdelay：若有写操作则立即执行，应与sync配合使用；
-  subtree：若输出目录是一个子目录，则nfs服务器将检查其父目录的权限；
-  no_subtree：即使输出目录是一个子目录，nfs服务器也不检查其父目录的权限，这样可以提高效率(默认设置)；

命令
~~~~

showmount -e [ipaddress] ： 在客户端上查看服务端的资源共享情况

exportfs -auv ：卸载所有共享目录

exportfs -arv ：重新共享所有目录

示例
----

假设一个Unix风格的场景，其中一台计算机（客户端）需要访问存储在其他机器上的数据（NFS
服务器）：

1. 服务端实现 NFS 守护进程, 默认运行 nfsd, 用来使得数据可以被客户端访问

2. 服务端系统管理员可以决定哪些资源可以被访问, 导出目录的名字和参数,
通常使用 /etc/exports 配置文件 和 exportfs 命令。

::

 /home/client1           192.168.0.101(rw,async,no_subtree_check)
 /var/www                192.168.0.101(rw,async,fsid=0,crossmnt,no_subtree_check,no_root_squash)

3. 服务端 安全-管理员 保证它可以组织和认证合法的客户端.

4. 服务端网络配置保证可以跟客户端透过 防火墙 进行协商.

5. 客户端请求导出的数据, 通常调用一个 mount 命令. (The client asks the
server (rpcbind) which port the NFS server is using, the client connects
to the NFS server (nfsd), nfsd passes the request to mountd)

::

 mount 192.168.0.100:/home/client1 /mnt/nfs/home/client1
 mount 192.168.0.100:/var/www /var/www

6. 如果一切顺利,
客户端的用户就可以通过已经挂载的文件系统查看和访问服务端的文件了.

7.启动时挂载NFS

::

 vi /etc/fstab
 192.168.0.100:/home/client1  /mnt/nfs/home/client1  nfs4   rsize=8192,wsize=8192,timeo=14,bg,_netdev 0 0
 192.168.0.100:/var/www  /var/www   nfs4   rsize=8192,wsize=8192,timeo=14,bg,_netdev 0 0

.. rubric:: 参考文献

.. [#] `Linux NFS服务器的安装与配置 <http://www.cnblogs.com/mchina/archive/2013/01/03/2840040.html>`_
