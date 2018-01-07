`Valgrind <http://valgrind.org/>`__\ 是一款用于内存调试、内存泄漏检测以及性能分析的软件开发工具。Valgrind这个名字取自北欧神话中英灵殿的入口。valgrind包含一个核心，它提供一个虚拟的CPU运行程序，还有一系列的工具，它们完成调试，剖析和一些类似的任务。valgrind是高度模块化的，所以开发人员或者用户可以给它添加新的工具而不会损坏己有的结构。

语法
----

valgrind [options] prog-and-args

--tool=：选择工具，默认为memcheck工具

-h：显示帮助信息

--help-debug:和--help相同，并且还能显示通常只有Valgrind的开发人员使用的调试选项。

-q：安静地运行，只打印错误信息。

-v更详细的信息, 增加错误数统计。

--trace-children=no|yes
Valgrind会跟踪到子进程中。这经常会导致困惑，而且通常不是你所期望的，所以默认这个选项是关闭的。
[no]

--track-fds=no|yes
跟踪打开的文件描述,Valgrind会在退出时打印一个打开文件描述符的列表。每个文件描述符都会打印出一个文件是在哪里打开的栈回溯，和任何与此文件描述符相关的详细信息比如文件名或socket信息。[no]

--time-stamp=no|yes 增加时间戳到LOG信息？ [no]

--log-fd=
指定Valgrind把它所有的消息都输出到一个指定的文件描述符中去。默认值2,
是标准错误输出(stderr)。注意这可能会干扰到客户端自身对stderr的使用,
Valgrind的输出与客户程序的输出将穿插在一起输出到stderr。 [2=stderr]

--log-file=
将输出的信息写入到filename.PID的文件里，PID是运行程序的进行ID

--log-socket=ipaddr:port
指定Valgrind输出所有的消息到指定的IP，指定的端口。当使用1500端口时，端口有可能被忽略。如果不能建立一个到指定端口的连接，Valgrind将输出写到标准错误(stderr)。这个选项经常和一个Valgrind监听程序一起使用。

--xml=yes 将信息以xml格式输出，只有memcheck可用

--error-limit=no|yes
当这个选项打开时，在总量达到10,000,000，或者1,000个不同的错误，Valgrind停止报告错误。这是为了避免错误跟踪机制在错误很多的程序下变成一个巨大的性能负担。
[yes]

--error-exitcode=
指定如果Valgrind在运行过程中报告任何错误时的退出返回值，有两种情况；当设置为默认值(零)时，Valgrind返回的值将是它模拟运行的程序的返回值。当设置为非零值时，如果Valgrind发现任何错误时则返回这个值。在Valgrind做为一个测试工具套件的部分使用时这将非常有用，因为使测试工具套件只检查Valgrind返回值就可以知道哪些测试用例Valgrind报告了错误。
[0=disable]

--gen-suppressions=yes|no|all
当设置为yes时，Valgrind将会在每个错误显示之后自动暂停并且打印下面这一行：----
Print suppression ? --- [Return/N/n/Y/y/C/c]
----这个提示的行为和--db-attach选项(见下面)相同。如果选择是，Valgrind会打印出一个错误的禁止条目，你可以把它剪切然后粘帖到一个文件，如果不希望在将来再看到这个错误信息。当设置为all时，Valgrind会对每一个错误打印一条禁止条目，而不向用户询问。这个选项对C++程序非常有用，它打印出编译器调整过的名字。注意打印出来的禁止条目是尽可能的特定的。如果需要把类似的条目归纳起来，比如在函数名中添加通配符。并且，有些时候两个不同的错误也会产生同样的禁止条目，这时Valgrind就会输出禁止条目不止一次，但是在禁止条目的文件中只需要一份拷贝(但是如果多于一份也不会引起什么问题)。并且，禁止条目的名字像;名字并不是很重要，它只是和-v选项一起使用打印出所有使用的禁止条目记录。
[no]

--db-attach=no|yes
[no]当这个选项打开时，Valgrind将会在每次打印错误时暂停并打出如下一行：----
Attach to debugger ? --- [Return/N/n/Y/y/C/c] ----
按下回车,或者N、回车，n、回车，Valgrind不会对这个错误启动调试器。按下Y、回车，或者y、回车，Valgrind会启动调试器并设定在程序运行的这个点。当调试结束时，退出，程序会继续运行。在调试器内部尝试继续运行程序，将不会生效。按下C、回车，或者c、回车，Valgrind不会启动一个调试器，并且不会再次询问。注意：--db-attach=yes与--trace-children=yes有冲突。你不能同时使用它们。Valgrind在这种情况下不能启动。

--db-command= [gdb -nw %f
%p]通过--db-attach指定如何使用调试器。默认的调试器是gdb.默认的选项是一个运行时扩展Valgrind的模板。
%f会用可执行文件的文件名替换，%p会被可执行文件的进程ID替换。

这指定了Valgrind将怎样调用调试器。默认选项不会因为在构造时是否检测到了GDB而改变,通常是/usr/bin/gdb.使用这个命令，你可以指定一些调用其它的调试器来替换。

给出的这个命令字串可以包括一个或多个%p
%f扩展。每一个%p实例都被解释成将调试的进程的PID，每一个%f实例都被解释成要调试的进程的可执行文件路径。

--input-fd= [default: 0, stdin]

使用--db-attach=yes和--gen-suppressions=yes选项，在发现错误时，Valgrind会停下来去读取键盘输入。默认地，从标准输入读取，所以关闭了标准输入的程序会有问题。这个选项允许你指定一个文件描述符来替代标准输入读取。

--max-stackframe= [default: 2000000]

栈的最大值。如果栈指针的偏移超过这个数量，Valgrind则会认为程序是切换到了另外一个栈执行。如果在程序中有大量的栈分配的数组，你可能需要使用这个选项。valgrind保持对程序栈指针的追踪。如果栈指针的偏移超过了这个数量，Valgrind假定你的程序切换到了另外一个栈，并且Memcheck行为与栈指

针的偏移没有超出这个数量将会不同。通常这种机制运转得很好。然而，如果你的程序在栈上申请了大的结构，这种机制将会表现得愚蠢，并且Memcheck将会报告大量的非法栈内存访问。这个选项允许把这个阀值设置为其它值。应该只在Valgrind的调试输出中显示需要这么做时才使用这个选项。在这种情况下，它会告诉你应该指定的新的阀值。普遍地，在栈中分配大块的内存是一个坏的主意。因为这很容易用光你的栈空间，尤其是在内存受限的系统或者支持大量小堆栈的线程的系统上，因为Memcheck执行的错误检查，对于堆上的数据比对栈上的数据要高效很多。如果你使用这个选项，你可能希望考虑重写代码在堆上分配内存而不是在栈上分配。

--run-libc-freeres=<yes|no> [default: yes]

GNU
C库(libc.so)，所有程序共用的，可能会分配一部分内存自已用。通常在程序退出时释放内存并不麻烦
--
这里没什么问题，因为Linux内核在一个进程退出时会回收进程全部的资源，所以这只是会造成速度慢。glibc的作者认识到这样会导致内存检查器，像Valgrind，在退出时检查内存错误的报告glibc的内存泄漏问题，为了避免这个问题，他们提供了一个__libc_freeres()例程特别用来让glibc释放分配的所有内存。因此Memcheck在退出时尝试着去运行__libc_freeres()。不幸的是，在glibc的一些版本中，__libc_freeres是有bug会导致段错误的。这在Red
Hat
7.1上有特别声明。所以，提供这个选项来决定是否运行__libc_freeres。如果你的程序看起来在Valgrind上运行得很好，但是在退出时发生段错误，你可能需要指定--run-libc-freeres=no来修正，这将可能错误的报告libc.so的内存泄漏。

--sim-hints=hint1,hint2,...

传递杂凑的提示给Valgrind，轻微的修改模拟行为的非标准或危险方式，可能有助于模拟奇怪的特性。默认没有提示打开。小心使用！目前已知的提示有：

l lax-ioctls:
对ioctl的处理非常不严格，唯一的假定是大小是正确的。不需要在写时缓冲区完全的初始化。没有这个，用大量的奇怪的ioctl命令来使用一些设备驱动将会非常烦人。

l enable-inner:打开某些特殊的效果，当运行的程序是Valgrind自身时。

--kernel-variant=variant1,variant2,...

处理系统调用和ioctls在这个平台的默认核心上产生不同的变量。这有助于运行在改进过的内核或者支持非标准的ioctls上。小心使用。如果你不理解这个选项做的是什么那你几乎不需要它。已经知道的变量有：

l bproc:
支持X86平台上的sys_broc系统调用。这是为了运行在BProc，它是标准Linux的一个变种，有时用来构建集群。

--show-emwarns=<yes|no> [default: no]

当这个选项打开时，Valgrind在一些特定的情况下将对CPU仿真产生警告。通常这些都是不引人注意的。

valgrind被设计成非侵入式的，它直接工作于可执行文件上，因此在检查前不需要重新编译、连接和修改你的程序。要检查一个程序很简单，只需要执行下面的命令就可以了

``valgrind --tool=tool_name  program_name``

比如我们要对ls -l命令做内存检查，只需要执行下面的命令就可以了

``valgrind --tool=memcheck ls -l``

不管是使用哪个工具，valgrind在开始之前总会先取得对你的程序的控制权，从可执行关联库里读取调试信息。然后在valgrind核心提供的虚拟CPU上运行程序，valgrind会根据选择的工具来处理代码，该工具会向代码中加入检测代码，并把这些代码作为最终代码返回给valgrind核心，最后valgrind核心运行这些代码。

标准工具
--------

memcheck 内存使用检测
~~~~~~~~~~~~~~~~~~~~~

memcheck探测程序中内存管理存在的问题。它检查所有对内存的读/写操作，并截取所有的malloc/new/free/delete调用。Memcheck
工具主要检查下面的程序错误：

1）使用未初始化的内存 (Use of uninitialised memory)

2）使用已经释放了的内存 (Reading/writing memory after it has been
free’d)

3）使用超过 malloc分配的内存空间(Reading/writing off the end of malloc’d
blocks)

4）对堆栈的非法访问 (Reading/writing inappropriate areas on the stack)

5）申请的空间是否有释放 (Memory leaks – where pointers to malloc’d
blocks are lost forever)

6）malloc/free/new/delete申请和释放内存的匹配(Mismatched use of
malloc/new/new [] vs free/delete/delete [])

7）src和dst的重叠(Overlapping src and dst pointers in memcpy() and
related functions)

适用于Memcheck工具的相关选项：

--leak-check=no|summary|full

要求对leak给出详细信息?
Leak是指，存在一块没有被引用的内存空间，或没有被释放的内存空间，如summary，只反馈一些总结信息，告诉你有多少个malloc，多少个free
等；如果是full将输出所有的leaks，也就是定位到某一个malloc/free。

--show-reachable=<yes|no>

当这个选项关闭时，内存泄漏检测器只显示没有指针指向的内存块，或者只能找到指向块中间的指针。当这个选项打开时，内存泄漏检测器还报告有指针指向的内存块。这些块是最有可能出现内存泄漏的地方。你的程序可能，至少在原则上，应该在退出前释放这些内存块。这些有指针指向的内存块和没有指针指向的内存块，或者只有内部指针指向的块，都可能产生内存泄漏，因为实际上没有一个指向块起始的指针可以拿来释放，即使你想去释放它。

--freelist-vol= [default: 5000000]

当客户程序使用free(C中)或者delete(C++)释放内存时，这些内存并不是马上就可以用来再分配的。这些内存将被标记为不可访问的，并被放到一个已释放内存的队列中。这样做的目的是，使释放的内存再次被利用的点尽可能的晚。这有利于memcheck在内存块释放后这段重要的时间检查对块不合法的访问。这个选项指定了队列所能容纳的内存总容量，以字节为单位。默认的值是5000000字节。增大这个数目会增加memcheck使用的内存，但同时也增加了对已释放内存的非法使用的检测概率。

--workaround-gcc296-bugs=<yes|no> [default: no]

当这个选项打开时，假定读写栈指针以下的一小段距离是gcc
2.96的bug，并且不报告为错误。距离默认为256字节。注意gcc
2.96是一些比较老的Linux发行版(RedHat
7.X)的默认编译器，所以你可能需要使用这个选项。如果不是必要请不要使用这个选项，它可能会使一些真正的错误溜掉。一个更好的解决办法是使用较新的，修正了这个bug的gcc/g++版本。

--partial-loads-ok=<yes|no> [default: no]

控制memcheck如何处理从地址读取时字长度，字对齐，因此哪些字节是可以寻址的，哪些是不可以寻址的。当设置为yes是，这样的读取并不抛出一个寻址错误。而是从非法地址读取的V字节显示为未定义，访问合法地址仍然是像平常一样映射到内存。设置为no时，从部分错误的地址读取与从完全错误的地址读取同样处理：抛出一个非法地址错误，结果的V字节显示为合法数据。注意这种代码行为是违背ISO
C/C++标准，应该被认为是有问题的。如果可能，这种代码应该修正。这个选项应该只是做为一个最后考虑的方法。

--undef-value-errors=<yes|no> [default: yes]

控制memcheck是否检查未定义值的危险使用。当设为yes时，Memcheck的行为像Addrcheck,
一个轻量级的内存检查工具，是Valgrind的一个部分，它并不检查未定义值的错误。使用这个选项，如果你不希望看到未定义值错误。

cachegrind 性能分析
~~~~~~~~~~~~~~~~~~~

cachegrind是一个cache剖析器。它模拟
CPU中的一级缓存I1,D1和L2二级缓存，能够精确地指出程序中
cache的丢失和命中。如果需要，它还能够为我们提供cache丢失次数，内存引用次数，以及每行代码，每个函数，每个模块，整个程序产生的指令数。

手动指定I1/D1/L2缓冲配置，大小是用字节表示的。这三个必须用逗号隔开，中间没有空格，例如：
valgrind --tool=cachegrind
--I1=65535,2,64你可以指定一个，两个或三个I1/D1/L2缓冲。如果没有手动指定，每个级别使用普通方式(通过CPUID指令得到缓冲配置，如果失败，使用默认值)得到的配置。

--I1=,,

指定第一级指令缓冲的大小，关联度和行大小。

--D1=,,

指定第一级数据缓冲的大小，关联度和行大小。

--L2=,,

指定第二级缓冲的大小，关联度和行大小。

helgrind多线程竞争
~~~~~~~~~~~~~~~~~~

helgrind查找多线程程序中的竞争数据。helgrind
寻找内存中被多个线程访问，而又没有一贯加锁的区域，这些区域往往是线程之间失去同步的地方，而且会导致难以发掘的错误。helgrind实现了名为"Eraser"
的竞争检测算法，并做了进一步改进，减少了报告错误的次数。

--private-stacks=<yes|no> [default: no]

假定线程栈是私有的。

--show-last-access=<yes|some|no> [default: no]

显示最后一次字访问出错的位置。

callgrind
~~~~~~~~~

Callgrind收集程序运行时的一些数据，函数调用关系等信息，还可以有选择地进行cache
模拟。在运行结束时，它会把分析数据写入一个文件。callgrind_annotate可以把这个文件的内容转化成可读的形式。

--heap=<yes|no> [default: yes]

当这个选项打开时，详细的追踪堆的使用情况。关闭这个选项时，massif.pid.txt或massif.pid.html将会非常的简短。

--heap-admin= [default: 8]

每个块使用的管理字节数。这只能使用一个平均的估计值，因为它可能变化。glibc使用的分配器每块需要4~15字节，依赖于各方面的因素。管理已经释放的块也需要空间，尽管massif不计算这些。

--stacks=<yes|no> [default: yes]

当打开时，在剖析信息中包含栈信息。多线程的程序可能有多个栈。

--depth= [default: 3]

详细的堆信息中调用过程的深度。增加这个值可以给出更多的信息，但是massif会更使这个程序运行得慢，使用更多的内存，并且产生一个大的massif.pid.txt或者massif.pid.hp文件。

--alloc-fn=

指定一个分配内存的函数。这对于使用malloc()的包装函数是有用的，可以用它来填充原来无效的上下文信息。(这些函数会给出无用的上下文信息，并在图中给出无意义的区域)。指定的函数在上下文中被忽略，例如，像对malloc()一样处理。这个选项可以在命令行中重复多次，指定多个函数。

--format=<text|html> [default: text]

产生text或者HTML格式的详细堆信息，文件的后缀名使用.txt或者.html。

一般用法:

``$valgrind --tool=callgrind ./sec_infod``

会在当前目录下生成callgrind.out.[pid], 如果我们想结束程序, 可以

``$killall callgrind``

然后我们可以用

| ``$callgrind_annotate --auto=yes callgrind.out.[pid] > log``
| ``$vi log``

massif–内存分析工具，统计进程使用的内存情况，包括堆、栈
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

堆栈分析器，它能测量程序在堆栈中使用了多少内存，告诉我们堆块，堆管理块和栈的大小。Massif能帮助我们减少内存的使用，在带有虚拟内存的现代系统中，它还能够加速我们程序的运行，减少程序停留在交换区中的几率。

lackey
~~~~~~

lackey是一个示例程序，以其为模版可以创建你自己的工具。在程序结束后，它打印出一些基本的关于程序执行统计数据。

--fnname= [default: \_dl_runtime_resolve()]

对函数计数。

--detailed-counts=<no|yes> [default: no]

对读取，存储和alu操作计数。
