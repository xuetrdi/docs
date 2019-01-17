================================
Emacs C++ Semantic Jump -- rtags
================================

Install rtags
-------------

.. code-block:: shell

    git clone --recursive git@github.com:Andersbakken/rtags.git
    cd rtags
    mkdir build && cd build
    cmake ..
    make
    sudo make install

Launch rtags and Generate Semantic Jump
---------------------------------------

.. code-block:: shell

    # 切换到build目录下
    # 启动服务器
    rdm &
    # 生成compile_commands.json编译选项
    cmake . -DCMAKE_EXPORT_COMPILE_COMMANDS=1
    # 生成语义跳转文件
    rc -J .

Using rtags Semantic Jump
-------------------------



