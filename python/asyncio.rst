asyncio - 异步 I/O, 事件循环，协程和任务
======================================

此模块为编写单线程并发代码使用协程提供基础架构，sockets 和其他资源的 I/O 多路复用，运行网络客户端和服务端，和其他相关的基元。以下是更详细的包内容列表：

* 各种系统具体实现的可插拔事件循环 ;
* 传输和协议抽象;
* 支持 TCP、 UDP、 SSL、 子进程管道，延迟调用，等等 （有些可能依赖于系统）;
* 一个模仿 concurrent.futures 模块但适合在事件循环中使用的 Future 类;
* 协程和任务基于 yield from，帮助以顺序的方式编写并发代码;
* 取消操作支持 Future 和协程;
* 单线程的协程间使用的同步基元 类似于 threading 模块里那些;
* 一个接口用于将工作传递到线程池，有时在你绝对，必须不得不使用一个阻塞 I/O 调用的库的时候。


::

import asyncio

async def hello():
    print("Hello world!")
    await asyncio.sleep(3)
    print("Hello again!")
    return "hello"

async def hello2():
    print("Hello world2!")
    print("Hello again2!")
    return "hello2"

def callback(future):
    print('Callback: ', future.result())


if __name__ == "__main__":
    loop = asyncio.get_event_loop()
    task = asyncio.gather(asyncio.ensure_future(hello()),asyncio.ensure_future(hello2()))
    task.add_done_callback(callback)
    loop.run_until_complete(task)
    print(task)
    loop.close()
