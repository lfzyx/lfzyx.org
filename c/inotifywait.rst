inotifywait
=============

inotifywait [OPTION] FILE

-r 循环监控

-m 保持运行

-e 指定要监控的事件

--exclude 排除文件

``inotifywait -rme modify,attrib,move,close_write,create,delete,delete_self path``