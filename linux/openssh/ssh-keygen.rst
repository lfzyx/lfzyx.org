ssh-keygen
============

ssh-keygen 用于生成、管理和转换认证密钥，包括 RSA 和 DSA 两种密钥。
密钥类型可以用 -t 选项指定。如果没有指定则默认生成用于SSH-2的RSA密钥。
ssh-keygen 还可以用来产生 Diffie-Hellman group exchange (DH-GEX) 中使用的素数模数。
参见模数和生成小节。
一般说来，如果用户希望使用RSA或DSA认证，那么至少应该运行一次这个程序，
在 ~/.ssh/identity, ~/.ssh/id_dsa 或 ~/.ssh/id_rsa 文件中创建认证所需的密钥。
另外，系统管理员还可以用它产生主机密钥。
通常，这个程序产生一个密钥对，并要求指定一个文件存放私钥，同时将公钥存放在附加了".pub"后缀的同名文件中。
程序同时要求输入一个密语字符串(passphrase)，空表示没有密语(主机密钥的密语必须为空)。
密语和口令(password)非常相似，但是密语可以是一句话，里面有单词、标点符号、数字、空格或任何你想要的字符。
好的密语要30个以上的字符，难以猜出，由大小写字母、数字、非字母混合组成。密语可以用 -p 选项修改。
丢失的密语不可恢复。如果丢失或忘记了密语，用户必须产生新的密钥，然后把相应的公钥分发到其他机器上去。
RSA1的密钥文件中有一个"注释"字段，可以方便用户标识这个密钥，指出密钥的用途或其他有用的信息。
创建密钥的时候，注释域初始化为"user@host"，以后可以用 -c 选项修改。

参数
------

-a trials 在使用 -T 对 DH-GEX 候选素数进行安全筛选时需要执行的基本测试数量。

-B 显示指定的公钥/私钥文件的 bubblebabble 摘要。

-b bits 指定密钥长度。对于RSA密钥，最小要求768位，默认是2048位。DSA密钥必须恰好是1024位(FIPS 186-2 标准的要求)。

-C comment 提供一个新注释

-c 要求修改私钥和公钥文件中的注释。本选项只支持 RSA1 密钥。 程序将提示输入私钥文件名、密语(如果存在)、新注释。

-D reader 下载存储在智能卡 reader 里的 RSA 公钥。

-e 读取OpenSSH的私钥或公钥文件，并以 RFC 4716 SSH 公钥文件格式在 stdout 上显示出来。
 该选项能够为多种商业版本的 SSH 输出密钥。

-F hostname 在 known_hosts 文件中搜索指定的 hostname ，并列出所有的匹配项。
 这个选项主要用于查找散列过的主机名/ip地址，还可以和 -H 选项联用打印找到的公钥的散列值。

-f filename 指定密钥文件名。

-G output_file 为 DH-GEX 产生候选素数。这些素数必须在使用之前使用 -T 选项进行安全筛选。

-g 在使用 -r 打印指纹资源记录的时候使用通用的 DNS 格式。

-H 对 known_hosts 文件进行散列计算。这将把文件中的所有主机名/ip地址替换为相应的散列值。
 原来文件的内容将会添加一个".old"后缀后保存。这些散列值只能被 ssh 和 sshd 使用。
 这个选项不会修改已经经过散列的主机名/ip地址，因此可以在部分公钥已经散列过的文件上安全使用。

-i 读取未加密的SSH-2兼容的私钥/公钥文件，然后在 stdout 显示OpenSSH兼容的私钥/公钥。 该选项主要用于从多种商业版本的SSH中导入密钥。

-l 显示公钥文件的指纹数据。它也支持 RSA1 的私钥。

 对于RSA和DSA密钥，将会寻找对应的公钥文件，然后显示其指纹数据。

-M memory 指定在生成 DH-GEXS 候选素数的时候最大内存用量(MB)。

-N new_passphrase 提供一个新的密语。

-P passphrase 提供(旧)密语。

-p 要求改变某私钥文件的密语而不重建私钥。程序将提示输入私钥文件名、原来的密语、以及两次输入新密语。

-q 安静模式。用于在 /etc/rc 中创建新密钥的时候。

-R hostname 从 known_hosts 文件中删除所有属于 hostname 的密钥。
 这个选项主要用于删除经过散列的主机(参见 -H 选项)的密钥。

-r hostname 打印名为 hostname 的公钥文件的 SSHFP 指纹资源记录。

-S start 指定在生成 DH-GEX 候选模数时的起始点(16进制)。

-T output_file 测试 Diffie-Hellman group exchange 候选素数(由 -G 选项生成)的安全性。

-t type 指定要创建的密钥类型。可以使用："rsa1"(SSH-1) "rsa"(SSH-2) "dsa"(SSH-2)

-U reader 把现存的RSA私钥上传到智能卡 reader

-v 详细模式。ssh-keygen 将会输出处理过程的详细调试信息。常用于调试模数的产生过程。

 重复使用多个 -v 选项将会增加信息的详细程度(最大3次)。

-W generator 指定在为 DH-GEX 测试候选模数时想要使用的 generator

-y 读取OpenSSH专有格式的公钥文件，并将OpenSSH公钥显示在 stdout 上。

示例
-----



::

 makessh-keygen

 Generating public/private rsa key pair.
 Enter file in which to save the key (/root/.ssh/id_rsa):
 Enter passphrase (empty for no passphrase):
 Enter same passphrase again:
 Your identification has been saved in /root/.ssh/id_rsa.
 Your public key has been saved in /root/.ssh/id_rsa.pub.
 The key fingerprint is:
 83:9b:5d:fe:e7:1a:15:c1:4d:d2:c3:20:8b:45:92:a7 root@localhost.localdomain
 The key's randomart image is:
 +--[ RSA 2048]----+
 |         .o+ o=+.|
 |         .+.o .=o|
 |         .o.  . .|
 |       . E     . |
 |      . S .   .  |
 |       + +   .   |
 |      o . . .    |
 |           . ..  |
 |            o+.  |
 +-----------------+

``chmod 400 id_rsa.pub`` 赋予公钥400权限

id_rsa即私钥，备份至本地后将服务器上的删除。

``service sshd restart`` 重启ssh服务