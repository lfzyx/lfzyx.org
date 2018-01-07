Nmap (Network Mapper)
是一款开放源代码的网络探测和安全审核的工具。它的设计目标是快速地扫描大型网络，当然用它扫描单个主机也没有问题。Nmap以新颖的方式使用原始IP报文来发现网络上有哪些主机，那些主机提供什么服务(应用程序名和版本)，那些服务运行在什么操作系统(包括版本信息)，它们使用什么类型的报文过滤器/防火墙，以及一堆其它功能。Nmap通常用于安全审核，许多系统管理员和网络管理员也用它来做一些日常的工作，比如查看整个网络的信息，
管理服务升级计划，以及监视主机和服务的运行。 [1]_

安装
----

`debian <debian>`__:

``# apt-get install nmap``

`centos <centos>`__:

``# yum install nmap``

语法
----

``nmap [Scan Type(s)] ``\ ***``[Options]``***\ `` {target specification}``

-v 显示更多信息

-A 启用操作系统和版本检测，脚本扫描和路由跟踪

-p 指定要扫描的端口

-sV 探测开放的端口，以确定服务/版本信息

示例
----

| ``# nmap -v -A -sV -p 1-100 google.com``
| ``Starting Nmap 6.00  at 2014-01-21 17:40 CST``
| ``Nmap scan report for google.com (173.194.127.40)``
| ``Host is up (0.31s latency).``
| ``Other addresses for google.com (not scanned): 173.194.127.39 173.194.127.37 173.194.127.32 173.194.127.33 173.194.127.41 173.194.127.36 173.194.127.38 173.194.127.35 173.194.127.46 173.194.127.34``
| ``rDNS record for 173.194.127.40: hkg03s10-in-f8.1e100.net``
| ``Not shown: 99 filtered ports``
| ``PORT   STATE SERVICE VERSION``
| ``80/tcp open  http    Google httpd 2.0 (GFE)``
| ``|_http-methods: No Allow or Public header in OPTIONS response (status code 405)``
| ``| http-robots.txt: 248 disallowed entries (15 shown)``
| ``| /search /sdch /groups /images /catalogs /catalogues ``
| ``| /news /nwshp /setnewsprefs? /index.html? /? /?hl=*& ``
| ``|_/addurl/image? /pagead/ /relpage/``
| ``|_http-title: 302 Moved``
| ``Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port``
| ``Device type: specialized|WAP|media device``
| ``Running (JUST GUESSING): Crestron 2-Series (86%), Netgear embedded (86%), Western Digital embedded (86%)``
| ``OS CPE: cpe:/o:crestron:2_series``
| ``Aggressive OS guesses: Crestron XPanel control system (86%), Netgear DG834G WAP or Western Digital WD TV media player (86%)``
| ``No exact OS matches for host (test conditions non-ideal).``
| ``Network Distance: 12 hops``
| ``Service Info: OS: Linux; CPE: cpe:/o:linux:kernel``
| ``OS and Service detection performed. ``
| ``Nmap done: 1 IP address (1 host up) scanned in 45.90 seconds``

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `29 Practical Examples of Nmap Commands for Linux System/Network
   Administrators <http://www.tecmint.com/nmap-command-examples/>`__
