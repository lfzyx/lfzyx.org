Openssh - 安全传输协议
======================

SSH全称Secure Shell [#]_ ，是一项创建在应用层和传输层基础上的安全协议，为计算机上的Shell（壳层）提供安全的传输和使用环境,利用SSH协议可以有效防止远程管理过程中的信息泄露问题。
OpenSSH（OpenBSD Secure Shell\) [#]_ 是使用SSH通过计算机网络加密通信的实现。它是取代由SSH Communications Security所提供的商用版本的开放源代码方案。目前OpenSSH是OpenBSD的子计划。

安装
----

``apt-get install openssh-server openssh-client``

组件
-------

.. toctree::
    :maxdepth: 2

    scp
    sshd
    ssh-keygen
    ssh-agent
    ssh-add
    ssh-keyscan



.. rubric:: 参考文献

.. [#] `Secure Shell - 維基百科，自由的百科全書 <https://zh.wikipedia.org/wiki/Secure_Shell>`_
.. [#] `OpenSSH - 維基百科，自由的百科全書 <https://zh.wikipedia.org/wiki/OpenSSH>`_
