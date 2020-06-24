.. _get_depth:

获取深度图像
==============

深度图像为SDK处理图像，获取深度图像需要 ``EnableDepthProcessor`` 开启深度计算，然后通过回调获取。

参考代码片段：

.. code-block:: c++

  auto m_pSDK = new CIMRSDK();
  MRCONFIG config = {0};
  config.bSlam = false;
  config.imgResolution = IMG_640;
  config.imgFrequency = 50;
  config.imuFrequency = 1000;

  m_pSDK->Init(config);
  std::queue<cv::Mat> depth_queue;
  int depth_count = 0;
  if (m_pSDK->EnableDepthProcessor()) {
    m_pSDK->RegistDepthCallback(
        [&depth_count, &depth_queue](double time, cv::Mat depth) {
          if (!depth.empty()) {
            depth.convertTo(depth, CV_16U, 1000.0);
            depth_queue.push(depth);
            ++depth_count;
          }
        });
  }
  auto &&time_beg = times::now();
  while (true) {
    if (!depth_queue.empty()) {
      cv::imshow("depth", depth_queue.front());
      depth_queue.pop();
    }
    char key = static_cast<char>(cv::waitKey(1));
    if (key == 27 || key == 'q' || key == 'Q') { // ESC/Q
      break;
    }
  }
  delete m_pSDK;
  return 0;

上述代码，用了 OpenCV 来显示图像。选中显示窗口时，按 ``ESC/Q`` 就会结束程序。

完整代码样例，请见 `_get_depth.cpp <https://github.com/indemind/IMSEE-SDK/blob/master/demo/get_depth.cpp>`_ 。

.. tip::

  预览深度图某区域的值，请见 `get_depth_with_region.cpp <hhttps://github.com/indemind/IMSEE-SDK/blob/master/demo/get_depth_with_region.cpp>`_ 。
