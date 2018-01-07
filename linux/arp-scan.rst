Arp-scan
=========

局域网arp扫描工具，会在本地网络发送arp包来收集地址。如果有多个MAC地址声称拥有相同的IP地址，那么这里就存在冲突。

安装
----

``# apt-get install arp-scan``

语法
----

``arp-scan [options] [hosts...]``

示例
----

``#arp-scan -I eth0 -l``

| ``Interface: eth0, datalink type: EN10MB (Ethernet)``
| ``Starting arp-scan 1.8.1 with 256 hosts (http://www.nta-monitor.com/tools/arp-scan/)``
| ``10.10.10.10    c8:3a:35:80:f9:de   Tenda Technology Co., Ltd.``
| ``10.10.10.75    04:7d:7b:43:14:9f   (Unknown)``
| ``10.10.10.90    00:30:48:d9:8a:32   Supermicro Computer, Inc.``
| ``10.10.10.100   c8:9c:dc:ff:7a:a9   (Unknown)``
| ``10.10.10.101   f8:0f:41:57:5f:c9   Wistron InfoComm(ZhongShan) Corporation``
| ``10.10.10.102   bc:ae:c5:6f:03:5e   ASUSTek COMPUTER INC.``
