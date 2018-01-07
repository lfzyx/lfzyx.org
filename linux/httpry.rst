Httpry
=======

捕获HTTP流量，并且将HTTP协议层的数据内容以可读形式列举出来。

安装
----

`` # apt-get install httpry``

语法
----

-b file 将数据包以二进制文件的形式保存下来

-d 后台守护

-f format 指定输出格式

-F force output flush

-i device 指定监听网卡

-l threshold specify a rps threshold for rate statistics

-m methods 监视指定的HTTP方法（如：GET，POST，PUT，HEAD，CONNECT）

-n count set number of HTTP packets to parse

-o file 将数据以可读的字符文件形式保存下来

-p disable promiscuous mode

-P file use custom PID filename when running in daemon mode

-q suppress non-critical output

-r file 读取所保存的HTTP数据包文件

-s run in HTTP requests per second mode

-t seconds specify the display interval for rate statistics

-u user 设置进程属主
