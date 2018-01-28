mysql-connector-c
===================

配置
-----
- xcode

::

 项目设置 - Build Settings - Search Paths - Header Search Paths 加上 /usr/local/mysql-connector-c/include
 项目设置 - Build Settings - Search Paths - Library Search Paths 加上 /usr/local/mysql-connector-c/lib
 项目设置 - Build Settings - Linking - Other Linker Flags 加上 /usr/local/mysql-connector-c/lib/libmysqlclient .dylib

- linux
``apt-get install libmysqlclient-dev``

- gcc
``gcc mysql_config --include mysql_config --libs main.c``

- API
http://dev.mysql.com/doc/refman/5.7/en/c-api.html

- 预处理函数
http://blog.csdn.net/leibniz_zsu/article/details/1627449#t2