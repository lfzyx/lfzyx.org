.. index:: Tripwire

Tripwire - 文件完整性检查
==========================

Tripwire 是一个文件完整性检查程序，用于监控指定文件是否变更，这些变更包含（但不限于）:

-  inode
-  时间戳
-  文件大小
-  文件权限和所有权属性
-  哈希值
-  文件类型

初始化时，Tripwire
扫描文件系统，把每个文件的校验值存储到数据库中，建立正常文件的基线，下一次扫描时，将文件的校验值和数据库中的校验值进行对比，如果校验值不一样，它会通过电子邮件警报或日志报告任何异常情况。

安装
------

``apt-get install tripwire``

安装期间需要配置密码，Site passphrase 用于加密 tw.cfg 和 tw.pol ，Local
passphrase 用于加密数据库和 report 文件

配置
-----

- tw.cfg 是 Tripwire 的加密配置文件，twcfg.txt 则是该文件未加密的状态
- tw.pol 是 Tripwire 的加密策略文件，twpol.txt 则是该文件未加密的状态
- local.key 是本地密钥，用于保护数据库
- site.key 是站点密钥，用于保护tw.cfg 和 tw.pol

使用
-----

调整策略文件tw.pol
^^^^^^^^^^^^^^^^^^
::

 vi  twpol.txt
 twadmin --create-polfile --polfile tw.pol -S site.key twpol.txt

显示策略文件

``twadmin --print-polfile``

调整配置文件tw.cfg
^^^^^^^^^^^^^^^^^^

::

 ``vi twcfg.txt``
 ``twadmin --create-cfgfile --cfgfile tw.cfg -S site.key twcfg.txt``

显示配置文件

``twadmin --print-cfgfile``

初始化数据库
^^^^^^^^^^^^

``tripwire --init``

初始化结束后，会在 /var/lib/tripwire/ 生成数据库

进行检测
^^^^^^^^^^

``tripwire --check``

检测结束后，会在/var/lib/tripwire/report/ 生成检测报告

读取检测结果
^^^^^^^^^^^

``twprint --print-report -r /var/lib/tripwire/report/*.twr``

更新数据库
^^^^^^^^^^

如果文件的变更是正常的，那么应该更新数据库来忽略这次变更

``tripwire --update -r /var/lib/tripwire/report/*.twr``

执行该命令之后，您就进入了一个编辑器。搜索所报告的文件名。所有侵害或更新都在文件名前面有一个
[x]。

如果您希望接受这些更改为正当的，则只需保存退出文件即可。Tripwire
不再报告此文件。如果您想要这个文件不被添加到数据库，那么请删除 'x'。

保存文件并退出编辑器时，若有数据库更新发生，会提示您输入密码以完成该过程。如果没有更新发生，那么
Tripwire 会通知您的，并且不需要输入密码。

更新策略文件tw.pol
^^^^^^^^^^^^^^^^^^

策略文件的后续更新应该采取如下形式

``tripwire --update-policy --secure-mode low twpol.txt``
