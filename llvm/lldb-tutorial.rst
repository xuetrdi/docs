==========================
LLDB调试工具使用和调试技巧
==========================

Command Structure
-----------------

命令形式: `<noun> <verb> [-options [option-value]] [argument [argument...]]`

- 选项和选项值都是空格分割的。
- 双引号用户保护参数中的空格。
- 反斜杠作为转义符号。
- 选项可以放在命令行的任何位置。
- 以`-`开头的选项
- 以`--`开头的选项
- 通过TAB自动补全
- 支持调试命令别名和传参


Setting Breakpoint(设置断点)
------------------------

* 对文件的某行设置断点(二者选一)

  breakpoint set --file foo.c --line 12

  breakpoint set -f foo.c -l 12

* 对函数设置断点

  breakpoint set --name foo

  breakpoint set -n foo

* 多个函数一次性设置断点

  breakpoint set --name foo --name bar

* 在C++方法上设置断点

  breakpoint set --method foo
  breakpoint set -M foo

* 在OC选择器上设置断点

  breakpoint set --shlib foo.dylib --name foo
  breakpoint set -s foo.dylib -n foo

  # 其中--shlib指定共享库

* 查看设置的断点
  
  breakpoint list

Setting Watchpoint(设置监视点)
------------------------------

* 查看全局变量

  watch set var global

* 查看监视点

  watch list 

线程调试
--------

* 运行线程直到断点

  thread continue

  thread step-in        // the same as gdb's "step"

  thread step-over      // the same as gdb's "next"

  thread step-out       // the same as gdb's "finish"

  thread step-inst      // the same as gdb's "stepi"

  thread step-over-inst // the same as gdb's "nexti"

  thread until 100

* 检查线程状态

  thread list 

  thread backtrace

  thread backtrace all

  thread select 2

Examining Stack Frame State(检查堆栈帧状态)
-------------------------------------------

* 检查帧栈参数和局部变量的方法

  frame variable

  frame variable var-name

  frame variable \*var-name

  frame variable &var-name

  frame variable argv[0]

* 切换帧栈

  frame select id

* expr

  expr var-name

