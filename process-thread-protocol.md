一个进程必定只能跑在一个CUP上
一个进程包括多个线程
一个线程可以包含多个协程
协程是单线程？？

但终究线程与协程是在一个CPU上跑，所以解决不了CPU密集型问题，但是可以解决I/O密集型。

多进程可以解决CPU密集型问题。