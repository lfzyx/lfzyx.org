SVN - 上一代版本控制系统
=======================

Apache
Subversion（简称SVN，svn），是一个开放源代码的版本控制系统，相对于的RCS、CVS，采用了分支管理系统，
它的设计目标就是取代CVS。互联网上越来越多的控制服务从CVS转移到Subversion。

单库与多库
----------

如果项目是相关的，并且有可能要共享数据，那么建议使用单库svn，代码可以很容易的在两个项目之间拷贝和移动，并且操作的历史记录会被保存下来。 [#]_

如果工程是完全不相关的，并且他们之间不可能共享数据，这样最好创建几个独立的完全不相关的版本库，利于安全管理。

配置单库SVN
-----------

建立SVN版本库目录
~~~~~~~~~~~~~~~~~

建立目录：mkdir -p ../repository

建立svn版本库：svnadmin create ../repository

在../repository下就可以看到conf, db,format,hooks, locks,
README.txt等文件，说明一个SVN库已经建立。

修改主配置
~~~~~~~~~~

vi ../repository/conf/svnserve.conf

内容修改为:

[general]

anon-access = none

auth-access = write

配置允许访问的svn用户
~~~~~~~~~~~~~~~~~~~~~

vi ../repository/conf/pwd

文件格式如下：

[users]

 =

 =

其中，[users]是必须的。下面列出要访问svn的用户，每个用户一行。

示例：

[users]

alan = password

king = hello

配置svn用户访问权限
~~~~~~~~~~~~~~~~~~~

vi ../repository/conf/authz

用户组：

[groups]

用户组a = 用户1,用户2 用户组b = 用户3,用户4

版本库目录：

[/目录]

@用户组 = 权限

用户 = 权限

用户组在前面加@，*表示全部用户。

权限可以是w、r、wr和空，空表示没有任何权限。

示例：

::

 [groups]
 harry_and_sally = harry,sally
 harry_sally_and_joe = harry,sally,&joe
 [/] #表示版本库根目录
 harry = rw
 [/目录] #表示版本库根目录下的目录
 @harry_and_sally = rw

*注意：对用户配置文件的修改立即生效，不必重启svn。权限配置文件中出现的用户名必须已在用户配置文件中定义。对权限配置文件的修改立即生效，不必重启svn*

启动单库svn
~~~~~~~~~~~

``svnserve -d -r ../repository``

-d表示以daemon方式（后台运行）运行，启动路径指向版本库所在的路径，这样在SVN客户端直接访问svn://ip地址 即可。

配置多库svn
-----------

与单库svn不同的是多库svn需要建立多个版本库，各个版本库的配置文件需要单独修改。大部分配置方式和单库svn相同，这里不再复述，需要注意的是配置svn用户访问权限和启动方式。

配置多库svn用户访问权限
~~~~~~~~~~~~~~~~~~~~~~~

vi ../repository*/conf/authz *#每个版本库都需要单独配置*

用户组：

[groups]

用户组a = 用户1,用户2

用户组b = 用户3,用户4

版本库目录：

[版本库:/目录]

@用户组 = 权限

用户 = 权限

用户组在前面加@，*表示全部用户。

权限可以是w、r、wr和空，空表示没有任何权限。

示例：

::

 [groups]
 harry_and_sally = harry,sally
 harry_sally_and_joe = harry,sally,&joe
 [repository*:/] #表示版本库根目录
 harry = rw
 [repository*:/目录] #表示版本库根目录下的目录
 @harry_and_sally = rw

启动多库svn
~~~~~~~~~~~

``svnserve -d -r ../``

-d表示以daemon方式（后台运行）运行，启动路径不要指向版本库，而是指向所有版本库的上一层级，
需要在SVN客户端访问svn://ip地址/版本库 ，才能访问到指定的版本库

备份还原
--------

备份
~~~~

``svnadmin dump ../repository > *.dump``

或

``svnadmin hotcopy ../repository > PATH``

还原
~~~~

``svnadmin load ../repository < *.dump``

镜像
----

svn不支持分布式开发，所以把svn版本库保存在一台服务器上是不安全的

::

 # 创建版本库
 svnadmin create PATH
 # 创建钩子
 cp PATH/hooks/pre-revprop-change.tmpl PATH/hooks/pre-revprop-change
 # 编辑pre-revprop-change钩子，将最后一行的 `exit 1` 改为 `exit 0`
 vi waukeen/hooks/pre-revprop-change
 # 给予钩子运行权限
 chmod +x PATH/hooks/pre-revprop-change
 # 为与另一个版本库的同步初始化目标版本库
 svnsync init file:///PATH svn://SOURCE_URL
 # 进行同步
 svnsync sync file:///PATH

.. rubric:: 参考文献

.. [#] `Subversion FAQ <http://subversion.apache.org/faq.zh.html>`_
