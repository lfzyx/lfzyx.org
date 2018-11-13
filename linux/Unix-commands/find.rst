find
=========

find 使用名称，大小，类型，文件所有者，创建时间，访问时间等搜索条件来搜索文件和目录

语法
-----

find [-H] [-L] [-P] [-D debugopts] [-Olevel] [path...] [expression]

示例
-----

* find . -name "\*python\*"  按名称查找文件

* find . -wholename "\*imghdrdata/\*python\*"  匹配完整路径

* find . -type d  查找文件夹

* find . -type f  查找文件

* find . -type l  查找符号链接

* find . -size +20M 查找超过指定大小的文件

* find . -size 13k 查找指定大小的文件

* find . -mtime -2 -type d 过去两天内修改的文件夹

* find . -atime -2 -type d 过去两天内访问过的文件夹

* find . -user root 查找 root 用户所拥有的文件

* find . -maxdepth 2 -type d ! -group root 查找不属于 root 组的文件夹，最大深度为2

* find . -type f -perm -o=rwx  查找 other 权限为 rwx 的文件

* find . -type f -perm -ug=rwx ! -perm -o=r 查找 user 和 group 拥有 rwx 权限，但 other 没有 r 权限的文件

* find . -type f -perm 600 查找 600 权限的文件