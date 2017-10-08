# asyncio并发编程

asyncio包使用事件循环驱动的协程实现并发。

并发编程的一种全新方式：yield from \+ 协程 \+ future \+ asyncio事件循环

对于事件循环来说，调用回调与在暂停的协程上调用send()方法效果差不多。同时协程也能避免回调地狱。

+ asyncio.coroutine
  - 只有驱动协程，协程才能做事
  - 驱动asyncio.coroutine装饰的协程有两种方法
    * 使用yield from
    * 把协程传给asyncio包中接收协程或者future的函数

+ future
  - 封装了待执完成的异步操作
  - 可放入队列
  - 得到结果后可以获取结果
  - 表示终将发生的事情，某件事会发生的唯一方式是执行的时间已经编排好了

+ asyncio.Future的方法：
  - asyncio.Future对象的结果通常使用yield from，既yield from future
  - add_done_callback():接收类型用的对象，future运行结束后调用执行可调用对象
  - remove_done_callback():
  - result():future运行结束后调用，返回可调用对象的结果
  - set_result():
  - exception():
  - set_exception():
  - cancel():
  - cancelled():
  - mro():
  - done():返回布尔值，表示可调用对象是否已经执行

+ asyncio.Task的方法：
  - 是asyncio.Future的子类
  - 用于包装协程
  - asyncio.Task与threading.Thread对象等效，是gevent的绿色线程
  - Task用于驱动协程，Thread对象用于调用可调用的对象
  - Task对象由asyncio.async()或者事件循环对象的create_task()创建
  - 获取到的Task对象已经排定好执行的时间，Thread调用start()启动执行
  - add_done_callback():
  - remove_done_callback():
  - result():
  - exception():
  - cancel():终止任务
  - cancelled():取消终止
  - get_stack():
  - print_stack():
  - current_task():
  - all_tasks():
  - mro():
  - done(): 

+ 获取Task的两种方式
  - asyncio.async(coro_or_future)
  - BaseEventLoop.create_task(coro)

+ 协程与future的区别
  - 已经编排好执行时间的异步操作，使用yield from获取future的结果
  - 因为使用yield from获取结果，所以future是否可以理解为一个编排好的协程
  - 所以future是封装了具体任务并且被编排好执行时间的协程？

## asyncio事件循环

+ asyncio.Semaphor类的方法：
  - Semaphor(count):实例化对象的时候传入计数器
  - acquire():让计数器递减，count\>0，不会阻塞，count=0，会阻塞协程，直到Semaphor调用release()
  - mro():
  - locked()：
  - release():让计数器递增
  
+ asyncio.get_event_loop()
  - 返回一个事件循环对象
+ asyncio.as_completed()
  - 获取一个future迭代器
  - 循环遍历future迭代器获取future，使用yield from future
  - 必须放在协程中调用

+ 事件循环对象的方法:
  - run_until_complete()
    * 接收协程参数
    * 把协程给事件循环
  - run_in_executor()
    * 与上一个方法的不同是，另外开一个新的线程
  - run_forever()
+ asyncio.wait()

+ 防止阻塞事件循环
  - I/O密集型，多线程应用的背后会释放GIL，不会阻塞
  - 如过客户代码与asyncio事件循环共用一个线程，因此如果阻塞，整个应用冻结
  - 解决办法:事件循环对象的run_in_executor()
  - asyncio的事件循环背后维护了一个ThreadPoolExecutor对象，使用run_in_executor
  - 

+ 小结：

有些函数必然会阻塞，但是为了让程序持续运行，有两种可行方案：
- 使用多个线程
- 异步调用，以回调或者协程的形式实现
异步系统能避免用户级线程的开销，比多线程系统并发更多连接的主要原因。

在asyncio包中，BaseEventLoop.create_task(),接收一个协程，给协程排定好执行事件返回一个Task实例。

+ 工作方式：
  1. 创建事件循环对象
  2. 创建协程的引用
  3. 把协程的引用传给事件循环对象执行
  4. 关闭事件循环


任务协程：执行具体任务
调度协程：调度任务协程,异步操作: 需要执行异步的具体任务
future的结果:最终得到的运行结果

任务协程传入某些函数之后(编排时间)，返回future，调度协程yield from future
调度进程传入事件循环对象。

Future和Task是排定好的协程表现，某些函数接收协程，返回Future或Task。

yield from future:返回future结果

yield from 协程:把控制权交给事件循环