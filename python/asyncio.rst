asyncio - 异步 I/O, 事件循环，协程和任务
========================================

CPU 的执行是顺序的，线程是操作系统提供的一种机制，允许我们在操作系统的层面上实现“并行”。
而协程则可以认为是应用程序提供的一种机制（用户或库来完成），允许我们在应用程序的层面上实现“并行”。

由于本质上程序是顺序执行的，要实现这种“并行”的假像，我们需要一种机制，来“暂停”当前的执行流，并在之后“恢复”之前的执行流。
这在操作系统及多线程/多进程中称为“上下文切换” (context switch)。
其中“上下文”记录了某个线程执行的状态，包括线程里用到的各个变量，线程的调用栈等。
而“切换”指的就是保存某个线程当前的运行状态，之后再从之前的状态中恢复。只不过线程相关的工作是由操作系统完成，而协程则是由应用程序自己来完成。

与线程不同的时，协程完成的功能通常较小，所以会有需求将不同的协程串起来，我们暂时称它为协程链 (coroutine chain)。 [#]_

此模块为使用协程编写单线程并发代码，sockets 和其他资源的 I/O 多路复用，运行网络客户端和服务端提供了基础架构。以下是更详细的包内容列表：

* 可用于多种特定系统具体实现的可插拔事件循环
* 传输和协议抽象
* 支持 TCP、 UDP、 SSL、 子进程管道，延迟通信以及其他的具体支持（有些可能是系统相关的)
* 一个模仿 concurrent.futures 模块但适合在事件循环中使用的 Future 类
* 协程和任务基于 yield from，帮助以顺序的方式编写并发代码
* 对 Future 和协程提供终止支持
* 单线程中，在协程间使用同步原语来模拟那些线程模块
* 一个用于传递工作的线程池的接口，当年你不得不使用一个阻塞 I/O 调用的库的时候使用


在使用 asyncio 模块进行异步编程前需要理解以下概念 [#]_ ：

* Event loop
  事件循环复用 I/O，利用 selectors 工作，序列化事件处理。
  程序开启一个无限的循环，把一些函数注册到事件循环上。当满足事件发生的时候，调用相应的协程函数。

* coroutine
  协程对象，指一个使用async def 关键字定义的函数，它的调用不会立即执行函数，而是会返回一个协程对象。
  协程不能直接运行, 需要把协程对象注册到事件循环，由事件循环调用。

* Futures
  这是那些延迟生产者的抽象。
  asyncio.Future 类与 Python3.2 中引入的 Future 类似。即，concurrent.futures.Future类。
  但是，在这种情况下，Future 适用于协程。该 asyncio 模块不适用现有的concurrent.futures.Future类，因为它被设计用于线程工作。
  该模块鼓励在协程中使用 await 锁住当前的任务来等待结果，从而避免阻塞你的应用。
  你的协程代码块；也就是说，你的协程被挂起，直到产生了结果，但是事件循环却没有被阻塞。
  若同一个 event loop 还有其他的任务序列，他们可能会运行。
  当协程产生结果的时候，暂停的协程会恢复，你可以编写同顺序执行一样的代码。
  你可以阅读代码，无需考虑 await 的存在。当你使用在函数中使用 await 返回一个 await 对象，你可以忘记 Future 执行的特殊的细节和它特定的 API。
  如果产生异常，比如你调用了函数，它没有返回 Future，却做了顺序执行，那么异常会被抛出。
  所以，编写异步代码同编写同步代码一样，除了添加 await。

* Tasks
  每个 Task 是一个被 Future 包裹的协程，随着 event loop 的运行而运行。
  asyncio.Task 类是 asyncio.Future 的子类。tasks 也与 await 一起工作。


语法 [#]_
-----------

* async def 用于定义协程的关键字

* await 挂起阻塞的异步调用接口

* asyncio.get_event_loop 创建一个默认的事件循环

* asyncio.gather 接受一堆协程

* asyncio.wait 接受一个协程组成的列表



下面是一个示例:

::

 import asyncio


 async def hello_world():  # 创建一个协程（async def），它是 Future 对象
     print("Hello World!")
     await asyncio.sleep(2)
     print("Bye World!")
     return "Hello World"


 async def hello_lfzyx():  # 创建一个协程（async def），它是 Future 对象
     print("Hello lfzyx!")
     await asyncio.sleep(3)
     print("Bye lfzyx!")
     return "lfzyx"


 def callback(re):
     print('Callback: ', re.result())


 if __name__ == "__main__":
     loop = asyncio.get_event_loop()  # 创建一个默认的事件循环
     task = asyncio.gather(hello_world(), hello_lfzyx())
     task.add_done_callback(callback)
     loop.run_until_complete(task)  # 将协程注册到事件循环，并启动事件循环
     loop.close()  # 关闭事件循环



.. rubric:: 参考文献

.. [#] `理解 Python asyncio <http://lotabout.me/2017/understand-python-asyncio/>`_
.. [#] `Python之asyncio <https://vvl.me/2016/03/python-coroutines/>`_
.. [#] `Python黑魔法 <https://www.jianshu.com/p/b5e347b3a17c>`_
.. [#] `使用Python进行并发编程 <http://www.dongwm.com/archives/使用Python进行并发编程-asyncio篇/>`_
