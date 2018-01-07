Tcpdump是最广泛使用的网络包分析器或者包监控程序之一，它用于捕捉或者过滤网络上指定接口上接收或者传输的TCP/IP包。它还有一个选项用于把捕捉到的包用pcap格式保存，可以用tcpdump命令和Wireshark工具进行分析。 [1]_

安装
----

``# apt-get install tcpdump``

语法
----

``# tcpdump ``\ ***``[OPTION]``***

-i 监听指定 interface.如果不指定接口, tcpdump 会捕获全部接口的数据包

-c 设置捕获数据包的次数

-A 用 ASCII 格式显示捕获的数据包

-D 列出tcpdump支持的端口

-w 将捕获到的包用pcap格式保存

-r 读取pcap格式文件

-n 显示数字形式地址而不是去解析主机

示例
----

只捕获eth0接口上的tcp数据包

``# tcpdump -i eth0 tcp``

只捕获端口80的数据包

``# tcpdump port 80``

只捕获源IP192.168.0.2的数据包

``# tcpdump src 192.168.0.2``

只捕获目的IP192.168.0.2的数据包

``# tcpdump dst 192.168.0.2``

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `12 Tcpdump Commands – A Network Sniffer
   Tool <http://www.tecmint.com/12-tcpdump-commands-a-network-sniffer-tool/>`__
