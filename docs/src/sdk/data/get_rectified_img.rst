.. _get_rectified_img:

获取矫正图像
==============

矫正图像为SDK处理图像，获取矫正图像需要 ``EnableRectifyProcessor`` 开启深度计算，然后通过回调获取。
参考代码片段：

.. code-block:: c++

  auto m_pSDK = new CIMRSDK();
  MRCONFIG config = {0};
  config.bSlam = false;
  config.imgResolution = IMG_640;
  config.imgFrequency = 50;
  config.imuFrequency = 1000;

  m_pSDK->Init(config);
  std::queue<cv::Mat> rectified_queue;
  int rectified_img_count = 0;
  if (m_pSDK->EnableRectifyProcessor()) {
    m_pSDK->RegistRectifiedImgCallback(
        [&rectified_img_count, &rectified_queue](double time, cv::Mat left,
                                                 cv::Mat right) {
          if (!left.empty() && !right.empty()) {
            cv::Mat img;
            cv::hconcat(left, right, img);
            rectified_queue.push(img);
            ++rectified_img_count;
          }
        });
  }
  auto &&time_beg = times::now();
  while (true) {
    if (!rectified_queue.empty()) {
      cv::imshow("rectified_img", rectified_queue.front());
      rectified_queue.pop();
    }
    char key = static_cast<char>(cv::waitKey(1));
    if (key == 27 || key == 'q' || key == 'Q') { // ESC/Q
      break;
    }
  }
  delete m_pSDK;
  return 0;

上述代码，用了 OpenCV 来显示图像。选中显示窗口时，按 ``ESC/Q`` 就会结束程序。

完整代码样例，请见 `_get_rectified_img.cpp <https://github.com/indemind/IMSEE-SDK/blob/master/demo/get_rectified_img.cpp>`_ 。

