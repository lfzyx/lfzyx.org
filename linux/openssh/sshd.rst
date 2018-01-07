sshd
======

配置
----
::

 Port 监听端口
 ListenAddress 监听地址
 Protocol 2 使用SSH Protocol 2
 HostKey /etc/ssh/ssh_host_ed25519_key
 HostKey /etc/ssh/ssh_host_rsa_key
 UsePrivilegeSeparation yes 使用权限分离
 KeyRegenerationInterval 3600
 ServerKeyBits 768
 SyslogFacility AUTH
 LogLevel [DEBUG] 日志级别
 LoginGraceTime 登录验证过程中，切断连接之前服务器需要等待的时间
 PermitRootLogin [no/yes] 允许root登录
 StrictModes yes
 MaxAuthTries 最大错误尝试的次数
 RSAAuthentication [no/yes] 允许RSA验证
 PubkeyAuthentication [no/yes]
 AuthorizedKeysFile %h/.ssh/* 公钥路径
 IgnoreRhosts yes
 RhostsRSAAuthentication no
 HostbasedAuthentication no
 IgnoreUserKnownHosts yes
 PermitEmptyPasswords [no/yes] 允许空密码
 ChallengeResponseAuthentication no 是否响应任何形式的认证
 PasswordAuthentication [no/yes] 允许口令验证
 KerberosAuthentication no
 KerberosGetAFSToken no
 KerberosOrLocalPasswd yes
 KerberosTicketCleanup yes
 GSSAPIAuthentication yes
 GSSAPICleanupCredentials yes
 X11Forwarding yes
 X11DisplayOffset 10
 PrintMotd no 设置登录后是否显示一些信息，即打印出/etc/motd文件的内容，考虑到安全可以设置为no。
 PrintLastLog 设置登录时打印最后一次登录记录
 TCPKeepAlive yes
 UseLogin no
 MaxStartups  2:50:3 设置从第2个尚未登录的连接开始，以50%的几率拒绝新连接，直到全部尚未登录的连接数达到3
 Banner 登录后的提示消息
 UsePAM 使用PAM来管理认证
 ClientAliveInterval 客户端超时
 ClientAliveCountMax 客户端超时最大次数
 UsePrivilegeSeparation 设置用户的权限
 Compression 设置是否可以是用压缩命令，可以设置为yes
 AllowUsers [user@host]允许的用户
 AllowGroups [group]允许的组
 DenyUsers [user@host]拒绝的用户

安全
----

TCP Wrappers
~~~~~~~~~~~~

使用TCP wrappers对sshd进行tcp包裹,只允许特定的主机才能连接到ssh服务端

::

 vi /etc/hosts.allow
 sshd:*.*.*.* 允许该IP SSH连接
 vi /etc/hosts.deny
 sshd:ALL 除了hosts.allow中允许的，其他IP都禁止SSH连接

PAM
~~~

利用 :doc:`../pam` 可做双因素登录认证 [#]_ [#]_

*注：key的优先级高于PAM，在key存在的情况下可以无视PAM认证直接用key登录，当然你也可以为key设置钩子command来解决这个问题*

knockd
~~~~~~

增强计算机安全性的最后一种方案是最激进的：关闭打开的端口，这会让任何攻击都无法攻破您的计算机. [#]_ 只向能够提供
“秘密敲门暗号” 的用户开放所需的端口，让用户能够输入密码并访问计算机。
敲门守护进程 knockd；它监视敲门序列，当发现有效的序列时执行相应的操作。

Web-based
~~~~~~~~~

通过web浏览器访问ssh服务  [#]_

.. rubric:: 参考文献

.. [#] `2-factor authentication & writing PAM modules for Ubuntu <http://ben.akrin.com/?p=1068>`_
.. [#] `google-authenticator <https://code.google.com/p/google-authenticator/>`_
.. [#] `保护 SSH 的三把锁 <http://www.ibm.com/developerworks/cn/aix/library/au-sshlocks/>`_
.. [#] `Web-based SSH <http://en.wikipedia.org/wiki/Web-based_SSH>`_
