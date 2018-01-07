scp
=====

scp(secure copy),用于主机间文件传输。scp基于ssh协议来传输及验证数据，提供与ssh相同的安全保护，会根据需要询问口令。 [#]_

语法
----

``scp [OPTION] [[user@]host1:]file1 ... [[user@]host2:]file2``

-P port

-S program

-o ssh_option

-l limit

-i identity_file

-F ssh_config

-c cipher

-r 递归复制整个目录

-v 显示进度

-C 允许压缩

示例
----

传输文件至远程主机

``scp com.dotgears.flappybird-1.apk root@192.168.1.1:/data/app/com.dotgears.flappybird-1.apk``

传输目录至ssh端口为2222的远程主机

``scp -P 2222 -r /usr/local/app/ root@192.168.1.1:/data/app/``

获取远程主机的目录

``scp -r root@192.168.1.1:/data/app/ /usr/local/app/``

获取ssh端口为2222的远程主机的文件

``scp -P 2222 root@192.168.1.1:/data/app/com.dotgears.flappybird-1.apk /usr/local/app/com.dotgears.flappybird-1.apk``


.. rubric:: 参考文献

.. [#] `Secure copy - Wikipedia <http://en.wikipedia.org/wiki/Secure_copy>`_
