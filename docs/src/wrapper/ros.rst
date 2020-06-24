.. _wrapper_ros:

ROS 如何使用
==============

按照 :ref:`sdk_install_ros_wrapper` ，编译再运行节点。

.. tip::

  使用下面命令前需要先在另一个命令行窗口运行ROS节点

``rostopic list`` 可以列出发布的节点：

.. code-block:: bash

  $ rostopic list
  /imsee/camera_info
  /imsee/depth
  /imsee/depth/compressed
  /imsee/depth/compressed/parameter_descriptions
  /imsee/depth/compressed/parameter_updates
  /imsee/depth/compressedDepth
  /imsee/depth/compressedDepth/parameter_descriptions
  /imsee/depth/compressedDepth/parameter_updates
  /imsee/depth/theora
  /imsee/depth/theora/parameter_descriptions
  /imsee/depth/theora/parameter_updates
  /imsee/disparity
  /imsee/disparity/compressed
  /imsee/disparity/compressed/parameter_descriptions
  /imsee/disparity/compressed/parameter_updates
  /imsee/disparity/compressedDepth
  /imsee/disparity/compressedDepth/parameter_descriptions
  /imsee/disparity/compressedDepth/parameter_updates
  /imsee/disparity/theora
  /imsee/disparity/theora/parameter_descriptions
  /imsee/disparity/theora/parameter_updates
  /imsee/image/camera_info
  /imsee/image/detector
  /imsee/image/detector/compressed
  /imsee/image/detector/compressed/parameter_descriptions
  /imsee/image/detector/compressed/parameter_updates
  /imsee/image/detector/compressedDepth
  /imsee/image/detector/compressedDepth/parameter_descriptions
  /imsee/image/detector/compressedDepth/parameter_updates
  /imsee/image/detector/theora
  /imsee/image/detector/theora/parameter_descriptions
  /imsee/image/detector/theora/parameter_updates
  /imsee/image/left
  /imsee/image/left/compressed
  /imsee/image/left/compressed/parameter_descriptions
  /imsee/image/left/compressed/parameter_updates
  /imsee/image/left/compressedDepth
  /imsee/image/left/compressedDepth/parameter_descriptions
  /imsee/image/left/compressedDepth/parameter_updates
  /imsee/image/left/theora
  /imsee/image/left/theora/parameter_descriptions
  /imsee/image/left/theora/parameter_updates
  /imsee/image/rectified/camera_info
  /imsee/image/rectified/left
  /imsee/image/rectified/left/compressed
  /imsee/image/rectified/left/compressed/parameter_descriptions
  /imsee/image/rectified/left/compressed/parameter_updates
  /imsee/image/rectified/left/compressedDepth
  /imsee/image/rectified/left/compressedDepth/parameter_descriptions
  /imsee/image/rectified/left/compressedDepth/parameter_updates
  /imsee/image/rectified/left/theora
  /imsee/image/rectified/left/theora/parameter_descriptions
  /imsee/image/rectified/left/theora/parameter_updates
  /imsee/image/rectified/right
  /imsee/image/rectified/right/compressed
  /imsee/image/rectified/right/compressed/parameter_descriptions
  /imsee/image/rectified/right/compressed/parameter_updates
  /imsee/image/rectified/right/compressedDepth
  /imsee/image/rectified/right/compressedDepth/parameter_descriptions
  /imsee/image/rectified/right/compressedDepth/parameter_updates
  /imsee/image/rectified/right/theora
  /imsee/image/rectified/right/theora/parameter_descriptions
  /imsee/image/rectified/right/theora/parameter_updates
  /imsee/image/right
  /imsee/image/right/compressed
  /imsee/image/right/compressed/parameter_descriptions
  /imsee/image/right/compressed/parameter_updates
  /imsee/image/right/compressedDepth
  /imsee/image/right/compressedDepth/parameter_descriptions
  /imsee/image/right/compressedDepth/parameter_updates
  /imsee/image/right/theora
  /imsee/image/right/theora/parameter_descriptions
  /imsee/image/right/theora/parameter_updates
  /imsee/imu
  /imsee/points
  /rosout
  /rosout_agg
  /tf_static
  ...

.. tip::

  除左右目原始图像及imu以外的节点，只有主动订阅才会发布，这是为了节省运算量。

``rostopic hz <topic>`` 可以检查是否有数据：

.. code-block:: bash

  $  rostopic hz /imsee/imu
  subscribed to [/imsee/imu]
  average rate: 991.195
    min: 0.000s max: 0.003s std dev: 0.00014s window: 970
  average rate: 989.429
    min: 0.000s max: 0.005s std dev: 0.00016s window: 1959
  average rate: 986.974
    min: 0.000s max: 0.008s std dev: 0.00024s window: 2944
  average rate: 987.813
    min: 0.000s max: 0.008s std dev: 0.00023s window: 3938
  ...

``rostopic echo <topic>`` 可以打印发布数据等。了解更多，请阅读 `rostopic <http://wiki.ros.org/rostopic>`_ 。

ROS 封装的文件结构，如下所示：

.. code-block:: none

  <IMSEE>/ros/
  ├─src/
  │  └─imsee_ros_wrapper
  │  ├── CMakeLists.txt
  │  ├── config
  │  │   └── settings.yaml
  │  ├── launch
  │  │   ├── display.launch
  │  │   └── start.launch
  │  ├── nodelet_plugins.xml
  │  ├── package.xml
  │  ├── rviz
  │  │   └── imsee.rviz
  │  └── src
  │      ├── wrapper_node.cc
  │      └── wrapper_nodelet.cc

其中 ``config/settings`` 里，可以配置图像的帧率分辨率与imu的频率。
