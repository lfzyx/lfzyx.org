siege
=====
siege 是一个开源的多线程 HTTP/HTTPS
压力／回归测试工具，用于评估WEB应用在压力下的承受能力。可以根据配置对一个WEB站点进行多用户的并发访问，记录每个用户所有请求过程的相应时间，并在一定数量的并发访问下重复进行。siege可以从您选择的预置列表中请求随机的URL
 [1]_

安装
----

``# apt-get install siege``

语法
----

-C 显示当前配置,配置文件为/etc/siege/siegerc

-v 显示详情

-c 模拟并发用户数量

-f 指定urls文件,默认为/etc/siege/urls.txt

-i 随机点击urls文件中的url链接

-t 测试持续时间

-d 点击每个urls的延时

-b 进行压力测试，不进行延时

-r 重复测试次数

url格式 [protocol://] [servername.domain.xxx] [:portnumber]
[/directory/file]

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `Siege Manual <http://www.joedog.org/siege-manual/>`__
