# Linux进程间通信：IPC

每个进程是一个独立的运行空间。进程与进程之间是彼此封闭，
不能直接交换数据。既然进程的独立空间不能相互访问，那么就需要
建立一块公共访问空间用来数据交换，当然这块数据交换区域不能是另一个
进程之类的，那么在哪里划分这么一块空间作为数据交换？
在内核的划分一块缓存区用于做数据交换。Linux系统应用程序属于用户态
底层服务属于内核态。

多进程：多个进程完成多任务

多进程的难点：进程间通信

进程间通信的方式及种类。
----------------------
1. 父子进程间的通信
   + 特点: 
     - 有亲缘关系
   + 建立通信的方式:
     - 父进程创建数据缓冲区
     - 父进程fork出子进程
     - 然后父进程向缓冲区中写数据，子进程从缓冲区读数据
   + 关键函数：
     - pipe(fd):创建内核态缓冲区
     - fork():fork出子进程
   + 通信方式:
     - 单向通信，一对一
2. 任意两个进程间通信
   + 特点:
     - 无需血缘关系
   + 建立通信方式:
     — 函数或者命令mkfifo
   + 关键函数
     - mkfifo()
   + 通信方式:
     - 一对多，多对一
3. 内存共享映射
   + 特点:
     - 高效
   + 建立通信方式:
   + 关键函数:
     - mmap()
   + 通信方式:
4. 消息队列
   - 特点:
     创建一个队列,确定队列容量,如果队列空或者满阻塞等待。
     如果队列不为空，则可以取出数据。
     队列中放什么？
     多个进程传入同一个共享队列
     一个进程向队列里写数据
     一个进程从队列里读数据
     一方写入队，一方读出队。
   - 建立方式:
   - 关键函数:
   - 通信方式:



多任务:
多任务的重点: 如何协作任务

多任务的执行方式：
顺序执行(流式处理)
并发执行

多进程通信的本质是:数据拷贝

多进程，多线程，协程


队列的直线流:不能一对多