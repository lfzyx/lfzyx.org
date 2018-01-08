asyncio - 异步 I/O, 事件循环，协程和任务
========================================

CPU 的执行是顺序的，线程是操作系统提供的一种机制，允许我们在操作系统的层面上实现“并行”。
而协程则可以认为是应用程序提供的一种机制（用户或库来完成），允许我们在应用程序的层面上实现“并行”。

由于本质上程序是顺序执行的，要实现这种“并行”的假像，我们需要一种机制，来“暂停”当前的执行流，并在之后“恢复”之前的执行流。
这在操作系统及多线程/多进程中称为“上下文切换” (context switch)。
其中“上下文”记录了某个线程执行的状态，包括线程里用到的各个变量，线程的调用栈等。
而“切换”指的就是保存某个线程当前的运行状态，之后再从之前的状态中恢复。只不过线程相关的工作是由操作系统完成，而协程则是由应用程序自己来完成。

与线程不同的时，协程完成的功能通常较小，所以会有需求将不同的协程串起来，我们暂时称它为协程链 (coroutine chain)。[#]_

此模块为使用协程编写单线程并发代码，sockets 和其他资源的 I/O 多路复用，运行网络客户端和服务端提供了基础架构。以下是更详细的包内容列表：

* 可用于多种特定系统具体实现的可插拔事件循环
* 传输和协议抽象
* 支持 TCP、 UDP、 SSL、 子进程管道，延迟通信以及其他的具体支持（有些可能是系统相关的)
* 一个模仿 concurrent.futures 模块但适合在事件循环中使用的 Future 类
* 协程和任务基于 yield from，帮助以顺序的方式编写并发代码
* 对 Future 和协程提供终止支持
* 单线程中，在协程间使用同步原语来模拟那些线程模块
* 一个用于传递工作的线程池的接口，当年你不得不使用一个阻塞 I/O 调用的库的时候使用


在使用 asyncio 模块进行异步编程前需要理解以下组件[#]_：

* Event loop（事件循环）

  事件循环复用 I/O， 序列化事件处理，而且以一种策略模式工作，这种策略模式对自定义平台与框架是非常灵活的。
  例如，Tornado，Twisted，以及 Gevent 可以与 asyncio 一起工作，也可以构建在 asyncio 的顶层。
  此外， asyncio 为每个平台选择了最佳的 I/O 机制。
  Unix 和 LInux 利用 selectors 工作，而基于 Windows 的系统使用 IOCP 工作（I/O completion ports 的简称）。

* Futures

  这是那些延迟生产者的抽象。
  asyncio.Future 类与 Python3.2 中引入的 Future 类似。即，concurrent.futures.Future类。
  但是，在这种情况下，Future 适用于协程。该 asyncio 模块不适用现有的concurrent.futures.Future类，因为它被设计用于线程工作。
  该模块鼓励在协程中使用 await 锁住当前的任务来等带结果，从而避免阻塞你的应用。
  你的协程代码块；也就是说，你的协程被挂起，直到产生了结果，但是事件循环却没有被阻塞。
  若同一个 event loop 还有其他的任务序列，他们可能会运行。
  当协程产生结果的时候，暂停的协程会恢复，你可以编写同顺序执行一样的代码。
  你可以阅读代码，无需考虑 await 的存在。当你使用在函数中使用 await 返回一个 await 对象，你可以忘记 Future 执行的特殊的细节和它特定的 API。
  如果产生异常，比如你调用了函数，它没有返回 Future，却做了顺序执行，那么异常会被抛出。
  所以，编写异步代码同编写同步代码一样，除了添加 await。

* Tasks

  每个 Task 是一个被 Future 包裹的协程，随着 event loop 的运行而运行。
  asyncio.Task 类是 asyncio.Future 的子类。你也可能猜到，tasks 也与 await 一起工作。

下面是一个 asyncio 示例:

::

 import asyncio


 async def hello_world(): #创建一个协程（async def），它是 Future 对象
     print("Hello World!")
     await asyncio.sleep(3)
     print("Bye World!")
     return "Hello World"


 async def hello_lfzyx(): #创建一个协程（async def），它是 Future 对象
     print("Hello lfzyx!")
     print("Bye lfzyx!")
     return "lfzyx"


 def callback(re):
     print('Callback: ', re.result())


 if __name__ == "__main__":
     loop = asyncio.get_event_loop() #创建一个默认的事件循环
     task = asyncio.gather(asyncio.ensure_future(hello_world()), asyncio.ensure_future(hello_lfzyx()))
     task.add_done_callback(callback)
     loop.run_until_complete(task) #运行 event loop 的方法
     loop.close() #关闭事件循环

.. rubric:: 参考文献

.. [#] `理解 Python asyncio <http://lotabout.me/2017/understand-python-asyncio/>`_
.. [#] `Python之asyncio <https://vvl.me/2016/03/python-coroutines/>`_