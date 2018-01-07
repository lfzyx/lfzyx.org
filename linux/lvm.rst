lvm
====

修改逻辑卷大小
--------------

扩大逻辑卷
~~~~~~~~~~

#. 先检查分区 e2fsck -f ***PATH***
#. 扩大逻辑卷 lvresize -L 120G ***PATH***
#. 将文件系统扩大 resize2fs ***PATH***

缩小逻辑卷
~~~~~~~~~~

#. 先检查分区 e2fsck -f ***PATH***
#. 将文件系统缩小 resize2fs ***PATH*** 50G
#. 缩小逻辑卷 lvresize -L 50G ***PATH***

`在 LVM中录制逻辑卷快照并恢复（第三部分） <http://linux.cn/article-4145-1-qqmail.html>`__
`在Linux中扩展/缩减LVM（第二部分） <http://linux.cn/article-3974-1.html>`__
`在Linux中使用LVM构建灵活的磁盘存储（第一部分） <http://linux.cn/article-3965-1.html>`__
