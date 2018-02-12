======================
使用CMake构建C/C++工程
======================

CMake是什么？
---------------------

CMake是一款可以跨平台管理组织项目的工具。CMake的使用CMakeLists.txt来管理项目。
CMakeLists.txt是由命令，注释，空格组成。#号后面的一行都为注释。
命令由名称，小括号，参数组成。参数之间用空格表示。

CMake能干什么？
--------------

CMake可以生成Makefile文件,可以完成项目的组织,构建,编译,测试,安装的设置。
配合编译器就可以完成项目的构建，编译，测试，安装等。

CMake怎么来管理项目
-------------------

1. 单个源文件的CMakeLists.txt

   .. code-block:: CMake
       
       # 需要的CMake的版本
       cmake_minimum_required(VERSION 3.10)
       # 项目名
       project(projectname)
       # 指定要源文件和编译后生成的可执行程序名字
       add_executable(ExecuteFileName main.c)

2. 同一个目录，多个源文件的CMakeLists.txt

   .. code-block:: CMake
       
       # 需要的CMake的版本
       cmake_minimum_required(VERSION 3.10)
       # 项目名
       project(projectname)
       # 查找当前目录下所有的源文件并保存到DIR_SRCS变量
       aux_source_directory(. DIR_SRCS)
       # 编译变量代表目录下的所有源文件，生成可执行文件
       add_executable(ExecuteFileName ${DIR_SRCS})

3. 多个目录(根目录下有子目录)，多个源文件
   
   根目录的CMakeLists.txt文件

   .. code-block:: CMake
       
       # 需要的CMake的版本
       cmake_minimum_required(VERSION 3.10)
       # 项目名
       project(projectname)
       # 查找当前目录下的所有源文件并保存到变量DIR_SRCS中
       aux_source_directory(. DIR_SRCS)
       # 添加根目录下的子目录sub1
       add_subdirectory(sub1)
       # 指定要源文件和编译后生成的可执行程序名字
       add_executable(ExecuteFileName main.c)
       # 添加子目录构建的静态库
       target_link_libraries(ExecuteFileName sub1)
    
    子目录的CMakeLists.txt文件

    .. code-block:: CMake
       
       # 查找当前目录下所有源文件,并保存到变量DIR_LIB_SRCS变量
       aux_source_directory(. DIR_LIB_SRCS)
       # 生成链接库
       add_library(sub1 ${DIR_LIB_SRCS})  

4. 根据条件条件宏是否链接sub1库,并且生成对应的头文件

   根目录的CMakeLists文件

   .. code-block:: CMake
       
       # 需要的CMake的版本
       cmake_minimum_required(VERSION 3.10)
       # 项目名
       project(projectname)
       # 加入一个配置头文件,设置动态头文件,根据config.h.in生成config.h
       configure_file(
       "${PROJECT_SOURCE_DIR}/config.h.in"
       "${PROJECT_BINARY_DIR}/config.h")
       # 设置是否链接外部库的开关
       option(USE_MYMATH "Use provided sub1" ON)
       # 是否链接外部库的判断条件
       if(USE_MYMATH)
       include_directories("${PROJECT_SOURCE_DIR}/sub1")
       add_subdirectory(sub1)
       set (EXTRA_LIBS ${EXTRA_LIBS} sub1)
       endif(USE_MYMATH)
       # 查找当前目录下所有的源文件并保存到DIR_SRCS变量
       aux_source_directory(. DIR_SRCS)
       # 指定要源文件和编译后生成的可执行程序名字
       add_executable(ExecuteFileName ${DIR_SRCS})
       # 链接外部库
       target_link_libraries(ExecuteFileName ${EXTRA_LIBS})

   编写config.h.in文件

   .. code-block:: CMake
       
       #cmakedefine USE_MYMATH

5. 程序安装

   子目录的CMakeLists.txt文件

   .. code-block:: CMake
       
       # 查找当前目录下所有源文件,并保存到变量DIR_LIB_SRCS变量
       aux_source_directory(. DIR_LIB_SRCS)
       # 生成链接库
       add_library(sub1 ${DIR_LIB_SRCS}) 
       # 指定安装路径
       install(TARGETS ExecuteFileName DESTINATION bin)
       install(FILES "${PROJECT_BINARY_DIR}/config.h" DESTINATION include)

   根目录的CMakeLists文件

   .. code-block:: CMake
       
       # 需要的CMake的版本
       cmake_minimum_required(VERSION 3.10)
       # 项目名
       project(projectname)
       # 加入一个配置头文件,设置动态头文件,根据config.h.in生成config.h
       configure_file(
       "${PROJECT_SOURCE_DIR}/config.h.in"
       "${PROJECT_BINARY_DIR}/config.h")
       # 设置是否链接外部库的开关
       option(USE_MYMATH "Use provided sub1" ON)
       # 是否链接外部库的判断条件
       if(USE_MYMATH)
       include_directories("${PROJECT_SOURCE_DIR}/sub1")
       add_subdirectory(sub1)
       set (EXTRA_LIBS ${EXTRA_LIBS} sub1)
       endif(USE_MYMATH)
       # 查找当前目录下所有的源文件并保存到DIR_SRCS变量
       aux_source_directory(. DIR_SRCS)
       # 指定要源文件和编译后生成的可执行程序名字
       add_executable(ExecuteFileName ${DIR_SRCS})
       # 链接外部库
       target_link_libraries(ExecuteFileName ${EXTRA_LIBS})
       # 指定安装路径
       install(TARGETS ExecuteFileName DESTINATION bin)
       install(FILES "${PROJECT_BINARY_DIR}/config.h" DESTINATION include)

6. 为程序添加测试

   根目录的CMakeLists文件

   .. code-block:: CMake
       
       # 需要的CMake的版本
       cmake_minimum_required(VERSION 3.10)
       # 项目名
       project(projectname)
       # 加入一个配置头文件,设置动态头文件,根据config.h.in生成config.h
       configure_file(
       "${PROJECT_SOURCE_DIR}/config.h.in"
       "${PROJECT_BINARY_DIR}/config.h")
       # 设置是否链接外部库的开关
       option(USE_MYMATH "Use provided sub1" ON)
       # 是否链接外部库的判断条件
       if(USE_MYMATH)
       include_directories("${PROJECT_SOURCE_DIR}/sub1")
       add_subdirectory(sub1)
       set (EXTRA_LIBS ${EXTRA_LIBS} sub1)
       endif(USE_MYMATH)
       # 查找当前目录下所有的源文件并保存到DIR_SRCS变量
       aux_source_directory(. DIR_SRCS)
       # 指定要源文件和编译后生成的可执行程序名字
       add_executable(ExecuteFileName ${DIR_SRCS})
       # 链接外部库
       target_link_libraries(ExecuteFileName ${EXTRA_LIBS})
       # 指定安装路径
       install(TARGETS ExecuteFileName DESTINATION bin)
       install(FILES "${PROJECT_BINARY_DIR}/config.h" DESTINATION include)
       
       # 启动测试
       enable_testing()
       # 测试数据
       add_test(test_run ExecuteFileName 5 2)
       # 测试信息是否正常显示
       add_test(test_usage ExecuteFileName)
       set_tests_properties(test_usage PROPERTIES PASS_REGULAR_EXPRESSION
       "Usage: .\* base exponent")
       # 测试5的平方
       add_test(test_5_2 ExecuteFileName 5 2)
       set_tests_properties(test_5_2
       PROJECT_BINARY_DIR PASS_REGULAR_EXPRESSION "is 25")

7. 添加gdb的Debug调试

   .. code-block:: CMake
   
       set(CMAKE_BUILD_TYPE "Debug")
       set(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -O0 -Wall -g -ggdb")
       set(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -O3 -Wall")

8. 添加版本号

   .. code-block:: CMake
   
       # 设置主版本号
       set(ExecuteFileName_VISION_MAJOR 1)
       # 设置副版本号
       set(ExecuteFileName_VISION_MINOR 0)

9. CMake的跨平台特性以及与各个平台编译器的转换方法