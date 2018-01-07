Unix和类Unix系统上的压缩打包工具，可以将多个文件合并为一个文件，打包后的文件名亦为“tar”。目前，tar文件格式已经成为POSIX标准，最初是POSIX.1-1988，目前是POSIX.1-2001。
本程序最初的设计目的是将文件备份到磁带上(tape archive)，因而得名tar。

语法
----

-c，--create 创建新的tar文件

-x，--extract，--get 解开tar文件

-t，--list 列出tar文件中包含的文件的信息

-r，--append 追加新的文件到tar文件中

-u，--update 用已打包的文件的较新版本更新tar文件

-A，--catenate，--concatenate 追加 tar 文件至归档

-d，--diff，--compare 将文件系统里的文件和tar文件里的文件进行比较

--delete
删除tar文件里的文件。注意，这个功能不能用于已保存在磁带上的tar文件！

-v，--verbose
列出每一步处理涉及的文件的信息，只用一个“v”时，仅列出文件名，使用两个“v”时，列出权限、所有者、大小、时间、文件名等信息。

-k，--keep-old-files 不覆盖文件系统上已有的文件

-f，--file [主机名:]文件名
指定要处理的文件名。可以用“-”代表标准输出或标准输入。

-P，--absolute-names 使用绝对路径

-j，--bzip2 调用bzip2执行压缩或解压缩。

-z，--gzip，--gunzip，--ungzip 调用gzip执行压缩或解压缩

-Z，--compress，--uncompress 调用compress执行压缩或解压缩

-C 指定解压缩该文件的目录

示例
----

``# tar xvf file.tar.xz``

解包

``# tar -cvpzf test.tar.gz --exclude=/test.tar.gz --exclude=/sys --exclude=/dev --exclude=/proc /``

打包整个根文件系统
