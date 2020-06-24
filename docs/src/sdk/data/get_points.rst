.. _get_points:

获取点云图像
==============

点云图像深度图像为SDK处理图像，获取深度图像需要 ``EnablePointProcessor`` 开启点云计算，然后通过回调获取

参考代码片段：

.. code-block:: c++

  auto m_pSDK = new CIMRSDK();
  MRCONFIG config = {0};
  config.bSlam = false;
  config.imgResolution = IMG_640;
  config.imgFrequency = 50;
  config.imuFrequency = 1000;

  m_pSDK->Init(config);
  std::queue<cv::Mat> points_queue;
  int points_count = 0;
  if (m_pSDK->EnablePointProcessor()) {
    // m_pSDK->EnableLRConsistencyCheck();
    // m_pSDK->SetDepthCalMode(DepthCalMode::HIGH_ACCURACY);
    m_pSDK->RegistPointCloudCallback(
        [&points_count, &points_queue](double time, cv::Mat points) {
          if (!points.empty()) {
            points_queue.push(points);
            ++points_count;
          }
        });
  }
  PCViewer pcviewer;
  auto &&time_beg = times::now();
  while (true) {
    if (!points_queue.empty()) {
      pcviewer.Update(points_queue.front());
      points_queue.pop();
    }
    char key = static_cast<char>(cv::waitKey(1));
    if (key == 27 || key == 'q' || key == 'Q') { // ESC/Q
      break;
    }
    if (pcviewer.WasStopped()) {
      break;
    }
  }
  delete m_pSDK;
  return 0;

上述代码，用了 `PCL <https://github.com/PointCloudLibrary/pcl>`_ 来显示点云。关闭点云窗口时，也会结束程序。

完整代码样例，请见 `_get_points.cpp <https://github.com/indemind/IMSEE-SDK/blob/master/demo/get_points.cpp>`_ 。

.. attention::

  准备好了 `PCL <https://github.com/PointCloudLibrary/pcl>`_ 库，编译教程样例时才会有此例子。如果 `PCL <https://github.com/PointCloudLibrary/pcl>`_ 库安装到了自定义目录，那么请打开 `demo/CMakeLists.txt <https://github.com/indemind/IMSEE-SDK/blob/master/demo/CMakeLists.txt>`_ ，找到 ``find_package(PCL)`` ，把 ``PCLConfig.cmake`` 所在目录添加进 ``CMAKE_PREFIX_PATH`` 。
