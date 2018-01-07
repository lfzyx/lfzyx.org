MySQL
=====


MySQL在过去由于性能高、成本低、可靠性好，已经成为最流行的开源数据库，因此被广泛地应用在Internet上的中小型网站中。

数据类型
--------

MySQL有三大类数据类型, 分别为数字、日期\时间、字符串,
这三大类中又更细致的划分了许多子类型 [1]_

数字类型
~~~~~~~~

-  整数: tinyint、smallint、mediumint、int、bigint

-  浮点数: float、double、real、decimal

日期\时间
~~~~~~~~~

date、time、datetime、timestamp、year

字符串类型
~~~~~~~~~~

-  字符串: char、varchar

-  文本: tinytext、text、mediumtext、longtext

-  二进制(可用来存储图片、音乐等): tinyblob、blob、mediumblob、longblob

语法
----

-  create

| ``create database database_name ``\ **``[``\ *``option``*\ ``]``**\ `` ; ``\ *``创建数据库``*
| ``create table table_name ``\ *``创建表``*
| ``(``
| ``column1 int not null auto_increment ,``
| ``column2 char(8) not null ,``
| ``primary key('column1')``
| ``) ; ``
| *``int``\ ````\ ``指定该列的数据类型为``\ ````\ ``int``*
| *``not``\ ````\ ``null``\ ````\ ``说明该列的值不能为空,``\ ````\ ``如果不指定该属性,``\ ````\ ``默认可为空``*
| *``auto_increment``\ ````\ ``需在整数列中使用,``\ ````\ ``其作用是在插入数据时若该列为``\ ````\ ``NULL,``\ ````\ ``MySQL将自动产生一个比现存值更大的唯一标识符值。在每张表中仅能有一个这样的值且所在列必须为索引列``*
| *``primary``\ ````\ ``key``\ ````\ ``表示该列是表的主键,``\ ````\ ``本列的值必须唯一,``\ ````\ ``MySQL将自动索引该列``*

-  show

| ``show [ global | session ] variables; 显示变量``
| ``show databases; 显示数据库``
| ``show tables; 显示已创建的表``
| ``show status; 显示数据库状态``
| ``show table status; 显示表状态``
| ``show processlist; 查看连接状态``
| ``show index from table_name; 显示表中索引``

-  insert

``insert ``\ **``[into]``**\ `` table_name ``\ **``[(column1,``\ ````\ ``column2,``\ ````\ ``column3,``\ ````\ ``...)]``**\ `` values (value1, value2, value3, ...); 插入数据``

-  select

| ``select column1 from table_name ``\ **``[where``\ ````\ ``column2``\ ````\ ``=``\ ````\ ``|``\ ````\ ``>``\ ````\ ``|``\ ````\ ``<``\ ````\ ``|``\ ````\ ``>``\ ````\ ``=``\ ````\ ``|``\ ````\ ``<``\ ````\ ``|``\ ````\ ``!=``\ ````\ ``|``\ ````\ ``is``\ ````\ ``[not]``\ ````\ ``null``\ ````\ ``|``\ ````\ ``in``\ ````\ ``|``\ ````\ ``like``\ ````\ ``|``\ ````\ ``or``\ ````\ ``|``\ ````\ ``and]``**\ ``; 查询数据``
| ``select count(*) from table_name; ``\ *``计算表内行数``*
| ``select * from table_name; ``\ *``列出表内所有内容``*
| ``select sum(DATA_LENGTH)/1024/1024  from TABlES ; ``\ *``计算数据库大小``*
| ``select field0, field1 from table where field0="content"; 列出表中field0,field1这两列，并且field0的值是"content"``
| ``select * from table where field0 !="content"; 列出表中所有记录，并且field0的值不是"content"``
| ``select * from table where field0 = "content0" and field1 = “content1”;列出表中field0值为content0，field1值为content1的所有数据``
| ``select * from table where field0 != "content0" and field1 = "content1";列出表中field0值不为content0，field1值为content1的所有数据``
| ``select version(), database() ; ``\ *``显示服务器的版本和当前的数据库名称``*
| ``select current_date() ; ``\ *``获取当前日期``*

-  update

``update table_name set column1 = value ``\ **``[where``\ ````\ ``column2``\ ````\ ``=``\ ````\ ``|``\ ````\ ``>``\ ````\ ``|``\ ````\ ``<``\ ````\ ``|``\ ````\ ``>``\ ````\ ``=``\ ````\ ``|``\ ````\ ``<``\ ````\ ``|``\ ````\ ``!=``\ ````\ ``|``\ ````\ ``is``\ ````\ ``[not]``\ ````\ ``null``\ ````\ ``|``\ ````\ ``in``\ ````\ ``|``\ ````\ ``like``\ ````\ ``|``\ ````\ ``or``\ ````\ ``|``\ ````\ ``and]``**\ ``; 更新数据``

-  delete

``delete from table_name ``\ **``[where``\ ````\ ``column``\ ````\ ``=``\ ````\ ``|``\ ````\ ``>``\ ````\ ``|``\ ````\ ``<``\ ````\ ``|``\ ````\ ``>``\ ````\ ``=``\ ````\ ``|``\ ````\ ``<``\ ````\ ``|``\ ````\ ``!=``\ ````\ ``|``\ ````\ ``is``\ ````\ ``[not]``\ ````\ ``null``\ ````\ ``|``\ ````\ ``in``\ ````\ ``|``\ ````\ ``like``\ ````\ ``|``\ ````\ ``or``\ ````\ ``|``\ ````\ ``and]``**\ ``; 删除数据``

-  drop

| ``drop table tables_name; 删除表``
| ``drop database database_name; 删除库``

-  alter

| ``alter table table_name rename table_name1; 重命名表``
| ``alter table table_name add column2 数据类型 [after column1]; 添加列``
| ``alter table table_name change column1 column2 数据类型; 修改列``
| ``alter table table_name drop column1; 删除列``
| ``alter table table_name engine = innodb; 转换表存储引擎``

-  mysqlshow -u -p -i *显示特定的数据库信息*

-  mysqldump

| ``mysqldump -u username -p  database |gzip > database.sql.gz 导出数据库``
| ``mysqldump -u username -p -d database > database.sql 导出数据库结构``

-  describe

``describe table_name; ``\ *``列出表结构``*

-  use database_name;\ *进入数据库*

| ``grant all privileges on database.* to 'username'@'host' identified by 'passwd'; 新建用户``
| ``revoke all privileges on database.* from 'username'@'localhost' identified by 'passwd'; 移除用户``

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `mysql数据类型 <http://www.cnblogs.com/zbseoag/archive/2013/03/19/2970004.html>`__
