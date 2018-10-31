=====================
bazel 中C/C++构建规则
=====================


使用标签(label)引用目标(target)

// 表示WORKSPACE所在路径。即工程的根目录。

同一个包中引用使用：`:target-name`
不同包引用使用：`//path/to/package:target-name`

- 如果target是rule target:

  `package`是包含`BUILD`文件的目录路径,`target-name`是`BUILD`中输出目标的`name`属性

- 如果target是file target:
  
  `path/to/package`是`package`的`root of the package`, `target-name`是目标文件的名称，
  包含其完整路径。
  

cc_binary
---------

Implicit输出目标

.. code-block:: bazel

    cc_binary(
      name = "",            # 必须参数，表示构建输出的二进制程序的名字
      srcs = ["",""],       # .h, .hpp, .c, .cc文件的源文件列表
      hdrs = ["",""],       # .h头文件的列表
      deps = ["",""],       # 链接库列表
      copts= ["",""],       # C++编译选项
      defines = ["",""],    # 增加编译(complier)选项到copts
      includes = ["",""],   # 增加编译(complier)选项到copts
      linkopts = ["",""],   # 增加选项到C++ linker
      linkshared = Boolean, # 创建一个共享库，linkopts=['-static'] and linkshared=True ->.a;linkstatic=1 and linkshared=True->.so
      linkstatic = Boolean, # 在静态文件下链接二进制
      malloc = "",          # default is @bazel_tools//tools/cpp:malloc
      nocopts = "",         # 从C++编译命令中移除改字符串匹配的命令
      stamp = Integer,      # default 0
      toolchains = labels,  # 一些规则制定的工具链，默认情况使用Make变量
      win_def_file = Labels # Windows
    )


cc_import
---------

允许用户导入预编译的C/C++库。

- 链接一个静态库

.. code-block:: bazel

    cc_import(
      name = "mylib",
      hdrs = ["hello.h"],
      static_library = "libmylib.a",
    )


- 链接一个动态库(Unix)

.. code-block:: bazel

    cc_import(
      name = "mylib",
      hdrs = ["mylib.h"],
      shared_library = "libmylib.so",
    )

- 链接一个动态或静态库(Unix)

.. code-block:: bazel

    cc_import(
      name = "mylib",
      hdrs = ["mylib.h"],
      static_library = "libmylib.a",
      shared_library = "libmylib.so",
    )

    # first will link to libmylib.a 
    cc_binary(
      name = "first",
      srcs = ["first.cc"],
      deps = [":mylib"],
      linkstatic = 1,
    )
    
    # second will link to libmylib.so
    cc_import(
      name = "second",
      srcs = ["second.cc"],
      deps = [":mylib"],
      linkstatic = 0,
    )


.. code-block:: bazel

    cc_import(
      name : "",                 # 必须参数
      hdrs : ["",""],            # 头文件列表
      alwayslink : Boolean,      # default 0
      interface_library = label, # 单个接口库 for linking the shared library
      shared_library = label,    # 单个预编译共享库，bazel确保它在运行时依赖于它的二进制可用
      static_library = label,    # 单个预编译的静态库
      system_provided = Boolean, # 如果为1，则表示系统提供时所需的共享库由系统提供。则应指定interface_library & shared_library==NULL
    )

cc_library
----------

头文件包含检查：

必须在cc\_\*规则的hdrs或srsc中声明构建中使用的所有头文件。这是强制执行的(enforced)。

当决定是否将头文件放入hdrs或srcs时，取决于是否希望此库的使用者能够直接包含它。这跟某些编程语言
中的public和private可见性(visibility)大致相同。

.. code-block:: bazel

    cc_library(
      name = "",            # 库名，必填选项
      deps = [],            # 要链接到二进制的其它库列表
      srcs = [],            # 处理以创建目标的C和C++文件列表。
      hdrs = [],            # 此库发布的文件列表将由依赖规则中源直接包含。
      alwayslink = Boolean, # 如果为1，任何直接或间接依赖于此库二进制文件将链接到srcs中列出文件的所有目标文件中。即使某些文件不包含二进制文件引用的符号
      defines = [],         # 添加到compile定义列表
      include_prefix = "",  # 要添加到此规则头文件的前缀(prefix)
      includes = [],        # 要添加到compile的`include`目录
      linkopts = [],        # Add flags to the C++ linker command.
      linkstatic = Boolean, # 链接二进制以静态模式
      nocopts = "",         # 要屏蔽的C++编译命令 
      strip_include_prefix = "",  # 从此规则中strip的前缀
      textual_hdrs = [],    # 此库发布的头文件列表，由依赖规则中源文件包含。
      toolchains = [],      # 
      win_def_file = label, #
    )


cc_proto_library
----------------

从\*.proto生成C++代码，deps必须指向proto_library规则。

.. code-block:: bazel

    proto_library(
      name = "",
      deps = [], # proto_library规则生成C++代码。
    )


.. code-block:: bazel 

    cc_library(
      name = "lib",
      deps = [":foo_cc_proto"],
    )
    
    cc_proto_library(
      name = "foo_cc_proto",
      deps = [":foo_proto"],
    )
    
    proto_library(
      name = "foo_proto",
    )


cc_test
-------

.. code-block:: bazel 

    cc_test(
      name = "",
      deps = [],            # 链接到二进制的其它库列表
      srcs = [],            # 源文件，头文件，或者生成的源文件
      copts= [],            # 增加到C++编译器的命令。
      defines = [],         # 增加到编译的定义
      includes = [],        # 增加到编译的`include`的目录列表
      linkopts = [],        # 增加到C++连接器的命令列表
      linkstatic = Boolean, # 默认情况下对cc_binrary是启用，其余是关闭的。
      malloc = label,       # 
      nocopts = "",         # 要删除的编译器选项。
      stamp = Integer,      # default 0
      toolchains = [],      # 
      win_def_file = label, # Windows
    )


fdo_prefetch_hints
------------------

在Workspace空间的绝对路径中指定FDO配置文件

.. code-block:: bazel 

    fdo_prefetch_hints(
      name = "",
      profile = label, # 指示配置文件的标签 "//path/to/hists:profile.afdo."
      
    )


fdo_profile
-----------

.. code-block:: bazel 

    fod_profile(
      name = "",
      absolute_path_profile = "", # FDO配置文件的绝对路径 /absolute/path/profile.zip
      profile = label,            # FDO配置的标签
      proto_profile = label,      # protobuf profile label
    )

