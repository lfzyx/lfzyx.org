Inotify - 文件系统变化通知
===========================
是一个 Linux内核2.6.13开始引入的文件变化通知机制，它监控文件系统，并且及时向专门的应用程序发出相关的事件警告，比如删除、读、写和卸载操作等。 [#]_

inotify C API
-------------

Inotify 提供 3 个系统调用  [#]_

inotify_init()
~~~~~~~~~~~~~~

在内核中创建 inotify 子系统的一个实例，失败则返回
-1，成功的话将返回一个文件描述符，调用 read() 等待警告。read()返回struct
inotify_event 事件结构:

::

 struct inotify_event
 {
  int      wd;       /* Watch descriptor */
  uint32_t mask;     /* Mask of events */
  uint32_t cookie;   /* Unique cookie associating related events (for rename(2)) */
  uint32_t len;      /* Size of name field */
  char     name[];   /* Optional null-terminated name */
 };

inotify_add_watch()
~~~~~~~~~~~~~~~~~~~

用于添加监视器。每个监视器必须提供一个路径名和相关事件列表，要监控多个事件，只需在事件之间使用逻辑操作符或
— C 语言中的管道线（|）操作符

``inotify_add_watch( fd, "/home/strike", IN_MODIFY | IN_CREATE | IN_DELETE );``

该调用会返回一个惟一的标识符，使用这个标识符更改或删除相关的监视器。如果调用失败返回
-1。

事件列表：

-  IN_ACCESS: 文件被访问;
-  IN_MODIFY: 文件被修改;
-  IN_ATTRIB: 文件属性被修改,如 chmod、chown、touch 等;
-  IN_CLOSE_WRITE: 可写文件被关闭;
-  IN_CLOSE_NOWRITE: 不可写文件被关闭;
-  IN_CLOSE: 文件被关闭,等同于(IN_CLOSE_WRITE \| IN_CLOSE_NOWRITE) ;
-  IN_OPEN: 文件被打开;
-  IN_MOVED_FROM: 文件被移走,如 mv ;
-  IN_MOVED_TO: 文件被移来,如 mv、cp ;
-  IN_MOVE: 文件被移动,等同于(IN_MOVED_FROM \| IN_MOVED_TO);
-  IN_CREATE: 创建新文件;
-  IN_DELETE: 文件被删除,如 rm ;
-  IN_DELETE_SELF: 自删除,即一个可执行文件在执行时删除自己;
-  IN_MOVE_SELF: 自移动,即一个可执行文件在执行时移动自己;
-  IN_ONESHOT: 仅监控一次事件;
-  IN_ONLYDIR: 只监控目录;
-  IN_UNMOUNT: 宿主文件系统被 umount;
-  IN_ALL_EVENTS: 以上所有事件;

inotify_rm_watch()
~~~~~~~~~~~~~~~~~~

删除一个监视器。

inotify-tools
-------------

Inotify 工具库提供监控文件系统活动的命令行工具  [#]_

``apt-get install inotify-tools``

该工具提供两个命令

* inotifywait 仅执行阻塞，等待 inotify 事件。
您可以监控任何一组文件和目录，或监控整个目录树（目录、子目录、子目录的子目录等等）。

``inotifywait -rme modify,attrib,move,close_write,create,delete,delete_self path``

* inotifywatch 收集关于被监视的文件系统的统计数据，包括每个 inotify 事件发生多少次。

``inotifywatch [OPTION] FILE``


.. rubric:: 参考文献

.. [#] `inotify - 维基百科，自由的百科全书 <https://zh.wikipedia.org/wiki/Inotify>`_
.. [#] `使用 inotify 监控文件系统的活动 <http://www.ibm.com/developerworks/cn/linux/l-ubuntu-inotify/>`_
.. [#] `Inotify: 高效、实时的Linux文件系统事件监控框架 <http://www.infoq.com/cn/articles/inotify-linux-file-system-event-monitoring>`_
