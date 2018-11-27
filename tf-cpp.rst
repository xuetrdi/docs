==============
C++ TensorFlow
==============

简单说明
--------

- MacOS 10.14
- TensorFlow 1.11

如果想要使用C API,构建tensorflow/libtensorflow.so;
如果想要使用C++ API,构建tensorflow/libtensorflow_cc.so;

构建或安装工具
--------------

.. code-block:: shell

    brew install autoconf
    brew install automake
    brew install libtool
    brew install curl
    brew install bazel
    brew install cmake
    brew install protobuf

构建TensorFlow
--------------

环境准备
""""""""

- Bazel 
- Numpy
- ProtocBuf

.. code-block:: shell

    git clone https://github.com/tensorflow/tensorflow
    cd tensorflow 
    ./configure
    bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
    bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
    pip install /tmp/tensorflow_pkg/tensorflow-\*.whl


构建动态库
----------

- Bazel
- ProtocBuf

环境准备
""""""""

.. code-block:: shell

    bazel build -c opt --copt=-mavx --copt=-mavx2 --copt=-mfma --copt=-msse4.2 --config=monolithic //tensorflow:libtensorflow_cc.so

构建OpenCV
----------

环境准备
""""""""

- CMake

.. code-block:: shell
   
    git clone https://github.com/opencv/opencv
    cd opencv && git checkout 3.4
    cd 
    git clone https://github.com/opencv/opencv_contrib
    cd opencv_contrib && git checkout 3.4
    cd /path/to/opencv && mkdir build && cd build 

    cmake -D CMAKE_BUILD_TYPE=RELEASE \
          -D CMAKE_INSTALL_PREFIX=/usr/local \
          -D INSTALL_C_EXAMPLES=ON \
          -D INSTALL_PYTHON_EXAMPLES=ON \
          -D WITH_TBB=ON \
          -D WITH_V4L=ON \
          -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules \
          -D BUILD_EXAMPLES=ON ..
    make -j4
    sudo make install 

配置OpenCV的opencv.pc的文件的pkgconfig文件。配置编译器编译的头文件，以及链接的动态库。

构建Eigen
---------

环境准备
""""""""

.. code-block:: shell

     `Eigen <https://github.com/eigenteam/eigen-git-mirror/releases>`_
     tar xf eigen-git-mirror-\*.tar.gz
     cd eigen-git-mirror
     mkdir build && cd build
     cmake ..
     make
     sudo make install

基于CMake构建TensorFlow应用
---------------------------

环境准备
""""""""

- CMake
- Eigen

.. code-block:: shell

    cp -r bazel-genfiles  /usr/local/include/tensorflow/
    cp -r tensorflow/cc   /usr/local/include/tensorflow/
    cp -r tensorflow/core /usr/local/include/tensorflow/
    cp -r third_party     /usr/local/include/tensorflow/

    cp bazel-bin/tensorflow/libtensorflow_cc.so /usr/local/lib/
    cp bazel-bin/tensorflow/libtensorflow_framework.so /usr/local/lib/

代码
""""

.. code-block:: c++

    #include <tensorflow/core/>


.. code-block:: cmake

    source

编写pkgconfig文件tensorflow.pc
""""""""""""""""""""""""""""""

.. code-block:: pc

    Name: tensorflow
    Description: TensorFlow pc file
    Version: 1.0
    Cflags: -I/usr/local/include 
    Libs: -L/usr/local/lib -ltensorflow

.. code-block:: shell

    mv /path/to/tensorflow.pc /usr/local/lib/pkgconfig/

配置命令行编译别名
""""""""""""""""""

.. code-block:: shell

    alias tf="g++ `pkg-config --libs --cflags tensorflow`" 
    # 增加到~/.bash_profile
    source ~/.bash_profile

编译tensorflow应用代码

.. code-block:: shell

    tf tf_example_main.cc

基于Bazel构建TensorFlow应用
---------------------------

环境准备
""""""""

- Bazel

.. code-block:: shell

    mkdir tensorflow/cc/model
    touch tensorflow/cc/model/model.cc
    touch tensorflow/cc/model/BUILD

编写model.cc
""""""""""""

.. code-block:: c++

    source

编写BUILD
"""""""""

.. code-block:: bazel

    load("//tensorflow:tensorflow.bzl", "tf_cc_binary")

    tf_cc_binary(
      name = "model",
      srcs = ["model.cc"],
      deps = [
        "//tensorflow/cc:gradients",
        "//tensorflow/cc:grad_ops",
        "//tensorflow/cc:cc_ops",
        "//tensorflow/cc:client_session",
        "//tensorflow/core:tensorflow"
      ],
    )

构建和运行应用
""""""""""""""

.. code-block:: shell

    bazel run -c opt //tensorflow/cc/models:model

检验结果
""""""""

已通过检验基于C++构建TensorFlow Lite应用
------------------------------

环境准备
""""""""


基于TensorFlow冻结模型文件构建OpenCV检测应用
--------------------------------------------


