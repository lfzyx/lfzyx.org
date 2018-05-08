OpenLDAP - LDAP 协议的开源实现
================================

`LDAP <https://en.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol>`_ 是轻型目录访问协议。LDAP
的基本单元是条目(entry)，条目由属性（attribute）组成，属性由
ObjectClasses 约束，ObjectClasses 和其关联的属性定义在 schema 中。

简介
-----

Entry
^^^^^

条目，也叫记录项，是LDAP中最基本的单元，就像是数据库中的一行记录。通常对LDAP的添加、删除、更改、检索都是以条目为基本对象的。 [#]_

每一个条目的开头都有一个唯一的标识名（distinguished Name ，DN），如
DN: cn=baby,ou=marketing,ou=people,dc=lfzyx,dc=org"
。通过DN的层次型语法结构，可以方便地表示出条目在LDAP树中的位置，通常用于检索。rdn指dn逗号最左边的部分，如cn=baby.
LDAP目录树的最顶部就是根，也就是所谓的“Base
DN”，如”dc=mydomain,dc=org”。

Attribute
^^^^^^^^^

每个条目都可以有很多属性（Attribute），比如常见的人都有姓名、地址、电话等属性。每个属性都有名称及对应的值。

LDAP为人员组织机构中常见的对象都设计了属性：

+------------------------+------+------------------+------------------+---------------+
| 属性                    | 别名 | 语法              | 描述             | 值(举例)       |
+========================+======+==================+==================+===============+
| commonName             | cn   | Directory String | 姓名              | lfzyx         |
+------------------------+------+------------------+------------------+---------------+
| surname                | sn   | Directory String | 姓               | zhou          |
+------------------------+------+------------------+------------------+---------------+
| organizationalUnitName | ou   | Directory String | 单位（部门）名称   | DevOps        |
+------------------------+------+------------------+------------------+---------------+
| telephoneNumber        |      | Telephone Number | 电话号码          | 911           |
+------------------------+------+------------------+------------------+---------------+
| objectClass            |      |                  | 内置属性          | inetOrgPerson |
+------------------------+------+------------------+------------------+---------------+

ObjectClass
^^^^^^^^^^^^

属性不是随便定义的，需要符合一定的规则，而这个规则可以通过schema制定。比如，如果一个entry没有定义在
inetorgperson 这个 schema 中的objectClass:
inetOrgPerson，那么就不能为它指定employeeNumber属性，因为employeeNumber是在inetOrgPerson中定义的。

ObjectClass是属性的集合，LDAP预想了很多人员组织机构中常见的对象，并将其封装成对象类。比如人员（person）含有姓（sn）、名（cn）、电话(telephoneNumber)、密码(userPassword)等属性，单位职工(organizationalPerson)是人员(person)的继承类，除了上述属性之外还含有职务（title）、邮政编码（postalCode）、通信地址(postalAddress)等属性。通过对象类可以方便的定义条目类型。每个条目可以直接继承多个对象类，这样就继承了各种属性。

Schema
^^^^^^^

对象类（ObjectClass）、属性类型（AttributeType）、语法（Syntax）分别约定了条目、属性、值。这些构成了模式(Schema)——对象类的集合。条目数据在导入时通常需要接受模式检查，它确保了目录中所有的条目数据结构都是一致的。

LDIF
^^^^^

LDIF（LDAP Data Interchange
Format，数据交换格式）是LDAP数据库信息的一种文本格式，用于数据的导入导出，每行都是“属性:
值”

安装
-----

OpenLDAP [#]_ 是 LDAP 的开源实现，由三个主要的部分：

* slapd - LDAP服务守护进程
* libraries - 实现LDAP协议的库
* ldap-utils - ldapsearch, ldapadd, ldapdelete, ldapmodify, etc.

OpenLDAP 将数据存储在后端数据库，默认使用 mdb 数据库。

``apt-get install slapd ldap-utils``

安装完成后需要对 slapd 重新配置

``dpkg-reconfigure slapd``

* Omit OpenLDAP server configuration? **No**

* DNS domain name? **lfzyx.org**

* Organization name? **lfzyx**

* Administrator password? **admin的密码**

* Database backend? **MDB**

* Remove the database when slapd is purged? **No**

* Move old database? **Yes**

* Allow LDAPv2 protocol? **No**

配置
----

OpenLDAP 的配置文件是 /etc/ldap/slapd.d/cn=config
目录中的ldif格式文件。 [#]_

LDIF
----

LDIF 文件用于操作或修改数据 [#]_

添加
^^^^^

添加一个Marketing部门

::

 vi add_entry.ldif

 dn: ou=Marketing, dc=example,dc=com
 objectclass: top
 objectclass: organizationalUnit
 ou: Marketing

 ldapmodify -a -xWD 'cn=admin,dc=lfzyx,dc=org' -f add_entry.ldif

修改
^^^^^

添加mail属性，修改sn的值，删除description属性

::

 vi modify_entry.ldif

 dn: cn=Pete Minsky,ou=Marketing,dc=example,dc=com
 changetype: modify
 add: mail
 mail: pminsky@example.com
 replace: sn
 sn: Minsky
 delete: description
 description: sx

 ldapmodify -xWD 'cn=admin,dc=example,dc=com' -f modify_entry.ldif

图形界面
---------

尽管可以通过命令行管理 LDAP 数据，但使用 `Apache Directory Studio <http://directory.apache.org/studio/>`_ 更方便


.. rubric:: 参考文献

.. [#] `LDAP服务器的概念和原理简单介绍 <http://seanlook.com/2015/01/15/openldap_introduction/>`_
.. [#] `OpenLDAP - Wikipedia, the free encyclopedia <https://en.wikipedia.org/wiki/OpenLDAP>`_
.. [#] `OpenLDAP(2.4.3x)服务器搭建及配置说明 <http://seanlook.com/2015/01/21/openldap-install-guide-ssl/#3-1-apt-get安装>`_
.. [#] `LDIF修改ldap记录或配置示例 <http://seanlook.com/2015/01/22/openldap_ldif_example/>`_
