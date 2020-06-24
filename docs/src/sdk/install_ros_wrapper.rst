.. _sdk_install_ros_wrapper:

ROS Wrapper 安装
================

.. only:: html

  ===============
  ROS Kinetic
  ===============
  |build_passing|
  ===============

  .. |build_passing| image:: https://img.shields.io/badge/build-passing-brightgreen.svg?style=flat

.. only:: latex

  ===============
  ROS Kinetic
  ===============
        ✓
  ===============


环境准备
--------

* `ROS <http://www.ros.org/>`_

ROS Kinetic (Ubuntu 16.04)
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

  $ wget https://raw.githubusercontent.com/oroca/oroca-ros-pkg/master/ros_install.sh && \
  chmod 755 ./ros_install.sh && bash ./ros_install.sh catkin_ws kinetic

编译代码
--------

.. code-block:: bash

  $ cd <IMSEE-SDK>  # <IMSEE-SDK> 为SDK具体路径
  $ make ros

运行节点
--------

.. code-block:: bash

  $ sudo su #开启权限模式
  $ source ros/devel/setup.bash
  $ roslaunch imsee_ros_wrapper start.launch

如果想通过 RViz 预览：

.. code-block:: bash

  $ sudo su #开启权限模式
  $ source ros/devel/setup.bash
  $ roslaunch imsee_ros_wrapper display.launch


