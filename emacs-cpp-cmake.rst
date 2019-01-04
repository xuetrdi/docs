==================================
Emacs Build C++ Project with Cmake
==================================


Configure Build Directory
-------------------------

HotKey SPC u 6 SPC c c  ----> build (make)
       SPC u 4 SPC c c  ----> check (make check)


Edit .dir-locals.el
-------------------

1. 指定构建目录


Using Cmake command
-------------------

1. 手动cmake,然后Emacs HotKey


保存文件时自动重新运行cmake,需要设置cmake-ide-build-dir
cmake-ide-build-dir设置的目录是cmake-ide-
通过cmake-compile-command设置自定义命令

使用.dir-locals.el设置cmake-ide-project-dir和cmake-ide-build-dir变量
使用绝对路径,如果cmake-ide-build-dir中存在compile_commands.json文件,它将于
CMake项目一样有效