# 使用concurrent.futures处理并发

- future的概念

1. future的概念

future是指一种对象，表示异步执行的操作。
这个概念作用很大，是concurrent.futures模块和asyncio包的基础。

在I/O密集型中，如果代码写的对，那么不管是使用那种并发策略，吞吐量都比依次执行的代码高的多。

future封装待完成的操作，可以放入队列，完成状态的可以查询得到结果。

通常情况下我们不应该创建future，而只能由并发框架实例化。

future表示终将发生的事情，而确定某件事发生的唯一方式是执行的时间已经排定。
因此，只有排定把某件事交给concurrent.futures.Executor子类处理时，
才会创建concurrent.futures.Future实例。
Executor.submit()方法的参数是一个可调用的对象，调用这个方法后会为
传入的可调用对象排期，并返回一个future。

Future.result()方法会阻塞调用方所在的线程，直到结果返回。

Future.add_done_callback()方法只有一个参数类型用的对象，future运行结束后会调用指定的可调用对象

Executor.map()使用future，返回一个迭代器，迭代器的next方法调用Future.result()方法。
得到结果并非future本身。

concurrent.futures.as_completed()接收一个future列表，返回一个迭代器，
在future运行结束后产出future。

GIL对I/O密集型处理无害。Python标准库中的所有I/O密集型函数都会释放GIL，
允许其它线程运行。尽管有GIL，Python线程还是能在I/O密集型应用中发挥作用。

- 使用concurrent.futures模块启动进行

concurrent.futures.ProcessPoolExecutor类把工作分配给多个Python
进程处理，因此如果需要做CPU密集型处理使用这个模块可以绕开GIL，利用
所有可用的CPU核心。

concurrent.futures.ThreadPoolExecutor和
concurrent.futures.ProcessPoolExecutor都实现了Exector接口，
因此可以轻松把线程方案转化为进程方案。

另外使用Python处理CPU密集型工作，应该使用PyPy解释器，比CPython快。

- 线程与多进程的替代方案
  - threading
  - multiprocessing
  - 其中multiprocessing还能解决协作进程遇到的最大挑战，在进程间传递数据




