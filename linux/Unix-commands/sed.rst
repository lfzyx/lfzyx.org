sed
======

sed（意为流编辑器，源自英语“stream editor”的缩写）是Unix常见的命令行程序。
sed用来把文档或字符串里面的文字经过一系列编辑命令转换为另一种格式输出。
sed通常用来匹配一个或多个正则表达式的文本进行处理，最基础的用法是字符串替换。

语法
------

``sed 's/character/character2/flag' file 将文件中的character替换为character2``

示例
-----
``sed 's/and/\&/g;s/^I/You/g' ahappychild.txt 将and替换为& ， 将每一行开头的I替换为You``
``sed -n '/2014-06/p' catalina.out |sed -n 1,5p 输出日志中包含2014-06的前5行``