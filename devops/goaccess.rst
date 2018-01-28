GoAccess - nginx 日志分析
=========================

GoAccess 是一个开源的 web
访问日志分析工具，为系统管理员提供快速可靠的可视化统计信息，包括访问概况、动态页面请求、静态页面请求、访客排名，访客使用的操作系统，访客使用的浏览器，来路域名，404
错误，搜索爬虫，搜索关键词等等 [#]_

安装
----

``apt-get install goaccess``


语法
----

``goaccess -f log_file``

-a 为选定的主机启用User-Agents列表

-f 设置日志文件路径


示例
----

``goaccess -f  FILE -a``

分析指定的日志文件

``zcat -f  FILE | goaccess -a > report.html``

使用管道处理所有的日志文件,并生成html报告

.. rubric:: 参考文献

.. [#] `GoAccess - Visual Web Log Analyzer <http://goaccess.prosoftcorp.com/>`_
