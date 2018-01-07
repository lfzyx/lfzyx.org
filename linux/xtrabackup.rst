Xtrabackup
=========

Xtrabackup是由 Percona 开发的一个开源软件，可实现对 InnoDB 的数据备份，支持在线热备份（备份时不影响数据读写），特点如下：

备份过程快速、可靠；
备份过程不会打断正在执行的事务；
能够基于压缩等功能节约磁盘空间和流量；
自动实现备份检验；
还原速度快；
XtraBackup 有两个工具：xtrabackup 和 innobackupex：

xtrabackup 本身只能备份 InnoDB 和 XtraDB ，不能备份 MyISAM；
innobackupex 本身是 Hot Backup 脚本修改而来，同时可以备份 MyISAM 和 InnoDB，但是备份 MyISAM 需要加读锁。