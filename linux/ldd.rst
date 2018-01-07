Ldd
====

显示可执行文件的共享库依赖情况

语法
----

ldd [OPTION] FILE

-v 显示详细信息
-d 进行 重定位(relocation), 而且 报告 缺少的 函数 (仅限于 ELF)
-r 对 数据目标 (data object) 和 函数 进行 重定位, 而且 报告 缺少的 数据目标 (仅限于 ELF)