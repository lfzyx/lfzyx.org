Restorecon
=========


恢复 SELinux 文件属性文件属性即恢复文件的安全上下文
用法：
     restorecon [-iFnrRv] [-e excludedir ] [-o filename ] [-f filename | pathname...]
参数：
-i：忽略不存在的文件。
-f：infilename 文件 infilename 中记录要处理的文件。
-e：directory 排除目录。
-R -r：递归处理目录。
-n：不改变文件标签。
-o outfilename：保存文件列表到 outfilename，在文件不正确情况下。
-v：将过程显示到屏幕上。
-F：强制恢复文件安全语境