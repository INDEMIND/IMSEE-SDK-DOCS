.. _get_detector:

获取物体检测图像
============================

物体检测图像为SDK处理图像，获取物体检测像需要 ``EnableDetectorProcessor`` 开启计算，然后通过回调获取。

参考代码片段：

.. code-block:: c++

  auto m_pSDK = new CIMRSDK();
  MRCONFIG config = {0};
  config.bSlam = false;
  config.imgResolution = IMG_640;
  config.imgFrequency = 50;
  config.imuFrequency = 1000;

  m_pSDK->Init(config);
  std::queue<cv::Mat> detector_queue;
  int detector_count = 0;
  std::string name[10] = {
      "BG",    "PERSON", "PET_CAT",   "PET_DOG", "SOFA",
      "TABLE", "BED",    "EXCREMENT", "WIRE",    "KEY",
  };
  m_pSDK->EnableDetectorProcessor();
  m_pSDK->RegistDetectorCallback(
      [&detector_count, &detector_queue, &name](DetectorInfo info) {
        if (!info.img.empty()) {
          detector_queue.push(info.img);
          ++detector_count;
          for (int i = 0; i < info.finalBoxInfo.size(); ++i) {
            BoxInfo &obj = info.finalBoxInfo[i];
          }
        }
      });
  auto &&time_beg = times::now();
  while (true) {
    if (!detector_queue.empty()) {
      cv::imshow("detector", detector_queue.front());
      detector_queue.pop();
    }
    char key = static_cast<char>(cv::waitKey(1));
    if (key == 27 || key == 'q' || key == 'Q') { // ESC/Q
      break;
    }
  }

  delete m_pSDK;
  return 0;

上述代码，用了 OpenCV 来显示图像。选中显示窗口时，按 ``ESC/Q`` 就会结束程序。

完整代码样例，请见 `_get_detector.cpp <https://github.com/indemind/IMSEE-SDK/blob/master/demo/get_detector.cpp>`_ 。
