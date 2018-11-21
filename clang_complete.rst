==============
Clang Complete
==============

我的环境
--------

- MacOS 10.14

安装clang_complete
------------------

.. code-block:: shell

    brew install clang_complete

下载cc_args.py脚本
------------------

.. code-block:: shell

    git clone https://github.com/vim-scripts/clang-complete.git
    cd clang-complete/bin
    cp cc_args.py /usr/local/bin

使用make生成.clang_complete文件
--------------------------------

.. code-block:: shell
    
    cd /porject/to/root/path
    mkdir build && cd build
    # 生成Makefile
    CXX='cc_args.py g++' cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local ..
    # 根据Makefile生成.clang_complete
    make
    # 汇总,排序,去重生成一个.clang_complete文件并移动到项目根目录下
    find . | ag clang_complete | xargs cat | sort | uniq > project/to/root/.clang_complete

其它小问题
----------

如果SPC h d v,输入 company-clang-arguments,如果显示为nil 增加syntax-checking layer,如果显示项目根目录下的.clang_complete文件中的内容则正确。
`(syntax-checking :variables syntax-checking-enable-by-default nil)`

如果在头文件下面出现红线在 .clang_complete文件中增加`-Wno-unused-parameter`到最后一行