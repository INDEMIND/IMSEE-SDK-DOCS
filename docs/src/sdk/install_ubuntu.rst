.. _sdk_install_ubuntu_src:

Ubuntu 安装
=====================

.. only:: html

  =============== ===============
  Ubuntu 16.04    Ubuntu 18.04
  =============== ===============
  |build_passing| |build_passing|
  =============== ===============

  .. |build_passing| image:: https://img.shields.io/badge/build-passing-brightgreen.svg?style=flat

.. only:: latex

  =============== ===============
  Ubuntu 16.04    Ubuntu 18.04
  =============== ===============
   ✓               ✓
  =============== ===============

.. tip::

  预安装项如下：

  ============= =====================================================================
  Linux         How to install required packages
  ============= =====================================================================
  Debian based  sudo apt-get install build-essential cmake git
  Red Hat based sudo yum install make gcc gcc-c++ kernel-devel cmake git
  Arch Linux    sudo pacman -S base-devel cmake git
  ============= =====================================================================

.. ::

  `Installation of System Dependencies <https://github.com/LuaDist/Repository/wiki/Installation-of-System-Dependencies>`_

获取代码
--------

.. code-block:: bash

  sudo apt-get install git
  git clone https://github.com/indemind/IMSEE-SDK.git

准备依赖
--------

.. code-block:: bash

  cd <IMSEE-SDK>  # <IMSEE-SDK> 为SDK具体路径
  make init

* `OpenCV <https://opencv.org/>`_

.. tip::

  * 如果需要安装ROS，可以不用安装OpenCV/PCL, 以防兼容性问题。SDK默认基于OpenCV3.3.1(ROS Kinetic自带OpenCV版本)编译。同时我们也提供基于OpenCV3.４.３编译的库，在<IMSEE-SDK>/lib/others文件夹中。如果客户想基于该版本编译，请在相应文件夹下替换。详细可咨询技术支持。
  * OpenCV 如何编译安装，请见官方文档 `Installation in Linux <https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html>`_ 。或参考如下命令：

  .. code-block:: bash

    [compiler] sudo apt-get install build-essential
    [required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
    [optional] sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev

    $ git clone https://github.com/opencv/opencv.git
    $ cd opencv/
    $ git checkout tags/3.4.３

    $ mkdir build
    $ cd build/

    $ cmake \
    -DCMAKE_BUILD_TYPE=RELEASE \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    \
    -DWITH_CUDA=OFF \
    \
    -DBUILD_DOCS=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_PERF_TESTS=OFF \
    ..

    $ make -j4
    $ sudo make install

编译代码
--------

.. tip::

  如果 OpenCV 安装到了自定义目录或想指定某一版本，编译前可如下设置路径,或者直接把该变量写到~/.bashrc：

  .. code-block:: bash

    # OpenCV_DIR is the directory where your OpenCVConfig.cmake exists
    export OpenCV_DIR=~/opencv/build

  不然， CMake 会提示找不到 OpenCV 。

* `MNN <https://github.com/alibaba/MNN>`_

.. tip::

  MNN依赖protobuf（使用3.0或以上版本），如未安装过protobuf，请参考如下命令：

  .. code-block:: bash

    [required] sudo apt-get install autoconf automake libtool

    $ git clone https://github.com/google/protobuf.git
    $ cd protobuf
    $ git submodule update --init --recursive
    $ ./autogen.sh
    $ ./configure
    $ make
    $ make check
    $ sudo make install
    $ sudo ldconfig # refresh shared library cache.
    $ protoc --version # 若安装成功，将显示protoc版本

  至此，可以开始编译安装MNN了：

  .. code-block:: bash

    $ git clone https://github.com/alibaba/MNN.git
    $ cd MNN
    $ ./schema/generate.sh
    $ mkdir build $$ cd build
    $ cmake ..
    $ make -j4
    $ sudo make install

安装好OpenCV与MNN后，可以开始编译SDK中的demo了

编译样例
--------

.. code-block:: bash

　　$ cd <IMSEE-SDK>  # <IMSEE-SDK> 为SDK具体路径
　　$ make demo

运行样例：

.. code-block:: bash

  $ ./demo/output/bin/get_image

教程样例，请阅读 :ref:`data`。

结语
----

工程要引入 SDK 的话，CMake 可参考 ``demo/CMakeLists.txt`` 里的配置。不然，就是直接引入安装目录里的头文件和动态库。
