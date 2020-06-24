.. _get_disparity_with_lr_check:

获取左右一致性检测视差图像
============================

视差图像为SDK处理图像，获取视差图像需要 ``EnableDisparityProcessor`` 开启视差计算，再通过``EnableLRConsistencyCheck``使能左右一致性检测（默认为不检测），然后通过回调获取。

参考代码片段：

.. code-block:: c++

  auto m_pSDK = new CIMRSDK();
  MRCONFIG config = {0};
  config.bSlam = false;
  config.imgResolution = IMG_640;
  config.imgFrequency = 50;
  config.imuFrequency = 1000;

  m_pSDK->Init(config);
  std::queue<cv::Mat> disparity_queue;
  int disparity_count = 0;
  if (m_pSDK->EnableDisparityProcessor()) {
    m_pSDK->EnableLRConsistencyCheck();
    // m_pSDK->SetDepthCalMode(DepthCalMode::HIGH_ACCURACY);
    m_pSDK->RegistDisparityCallback(
        [&disparity_count, &disparity_queue](double time, cv::Mat disparity) {
          if (!disparity.empty()) {
            disparity.convertTo(disparity, CV_8U, 255. / (16 * 64));
            cv::applyColorMap(disparity, disparity, cv::COLORMAP_JET);
            disparity.setTo(0, disparity == -16);
            disparity_queue.push(disparity);
            ++disparity_count;
          }
        });
  }
  auto &&time_beg = times::now();
  while (true) {
    if (!disparity_queue.empty()) {
      cv::imshow("disparity", disparity_queue.front());
      disparity_queue.pop();
    }
    char key = static_cast<char>(cv::waitKey(1));
    if (key == 27 || key == 'q' || key == 'Q') { // ESC/Q
      break;
    }
  }
  delete m_pSDK;
  return 0;

上述代码，用了 OpenCV 来显示图像。选中显示窗口时，按 ``ESC/Q`` 就会结束程序。

完整代码样例，请见 `_get_disparity_with_lr_check.cpp <https://github.com/indemind/IMSEE-SDK/blob/master/demo/get_disparity_with_lr_check.cpp>`_ 。

.. tip::

  开启左右一致性检测会影响视差计算的速度，用户酌情使用。
