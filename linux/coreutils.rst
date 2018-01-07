Coreutils - GNU核心工具组
==========================

GNU核心工具组（GNU Core
Utilities，亦常缩写为Coreutils）是一个包含了多个类Unix所需的基本工具的软件包，其亦是之前许多类似软件包（如textutils（文本工具组）、shellutils（shell工具组）、fileutils（文件工具组）等）所包含工具的集合。

df
----
df可以查看一级文件夹大小、使用比例、档案系统及其挂入点。
-h：表示使用「Human-readable」的输出，也就是在档案系统大小使用 GB、MB 等易读的格式。
界面相关说明
Filesystem：档案系统
Mounted on：挂入点
Size：分区容量
Used：已使用的大小
Avail：剩下的大小
Use%：使用的百分比
dfc 增强版df,可读性会更强

du
----
查询文件或文件夹的磁盘使用空间
--max-depth=指定深入目录的层数
ncdu更好用啦