Linux系统

+ 用户态与内核态通信的方式：
  - 文件读写:Procfs,Sysfs等
  - loctl:I/O Control
  - Socket
  - 系统调用:用户态应用程序调用内核态模块
  - 反向调用:内核态模块调用用户态应用
  - 信号:发送信号，发送消息，等待消息
  - 内存映射:映射出一块共享内存