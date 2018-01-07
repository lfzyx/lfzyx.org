shc是一款通用的shell脚本编译器,将shell脚本转换为二进制可执行文件 [1]_。shc的主要用途是保护shell脚本避免被修改和阅读。

安装
----

| ``# wget ``\ ```http://www.datsi.fi.upm.es/~frosal/sources/shc-3.8.9.tgz`` <http://www.datsi.fi.upm.es/~frosal/sources/shc-3.8.9.tgz>`__\ `` ``\  [2]_
| ``# tar zxvf  shc-3.8.9.tgz``
| ``# cd shc-3.8.9``
| ``# mv shc-3.8.9.c shc.c``
| ``# mkdir -p /usr/local/man/man1/``
| ``# make``
| ``# make install``

语法
----

``shc ``\ ***``[OPTION]``***\ `` -f ``\ ***``FILE``***\ `` ``\  [3]_

-e date 二进制可执行文件的过期时间，格式为 日/月/年

-m message 过期后的提示信息

-f script_name 指定要编译的shell脚本的路径及文件名

-i inline_option 内联选项

-x comand eXec command, as a printf format i.e: exec('%s',@ARGV)

-l last_option Last shell option i.e: --

-r 制作可再发行的二进制文件，使二进制文件能在多种相同的系统上执行

-v verbose模式，输出编译的详细情况

-D Switch on debug exec calls

-T 允许二进制文件可被追溯

-C 显示许可证

-A 显示摘要

-h 显示帮助

示例
----

``# shc -e 20/10/2018 -m "lfzyx.org" -v -T -r -f backup.sh``

运行后会生成两个文件,其中backup.sh.x是加密后的可执行二进制文件，backup.sh.x.c是c语言源文件，只需要把二进制文件传到服务器上即可。

参考文献
--------

.. raw:: html

   <references/>

.. [1]
   `Francisco Javier Rosales
   García <http://www.datsi.fi.upm.es/~frosal/>`__

.. [2]
   ```sources`` <http://www.datsi.fi.upm.es/~frosal/sources/>`__

.. [3]
   ```Manpage``\ ````\ ``for``\ ````\ ``shc(1)`` <http://www.datsi.fi.upm.es/~frosal/sources/shc.html>`__
