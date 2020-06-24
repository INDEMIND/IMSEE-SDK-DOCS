.. _get_imu:

获取imu数据
==============

获取imu前，需要初始化SDK,然后直接通过回调获取。

参考代码片段：

.. code-block:: c++

  auto m_pSDK = new CIMRSDK();
  MRCONFIG config = {0};
  config.bSlam = false;
  config.imgResolution = IMG_640;
  config.imgFrequency = 50;
  config.imuFrequency = 1000;

  m_pSDK->Init(config);

  std::queue<cv::Mat> image_queue;

  int img_count = 0;
  m_pSDK->RegistImgCallback(
      [&img_count, &image_queue](double time, cv::Mat left, cv::Mat right) {
        if (!left.empty() && !right.empty()) {
          cv::Mat img;
          cv::hconcat(left, right, img);
          image_queue.push(img);
          ++img_count;
        }
      });
  int imu_count = 0;
  m_pSDK->RegistModuleIMUCallback([&imu_count](ImuData imu) {
    if (imu_count % 100 == 0) {
      LOG(INFO) << "imu timestamp: " << imu.timestamp
                << ", accel x: " << imu.accel[0]
                << ", accel y: " << imu.accel[1]
                << ", accel z: " << imu.accel[2] << ", gyro x: " << imu.gyro[0]
                << ", gyro y: " << imu.gyro[1] << ", gyro z: " << imu.gyro[2];
    }
    ++imu_count;
  });
  auto &&time_beg = times::now();
  while (true) {
    if (!image_queue.empty()) {
      cv::imshow("image", image_queue.front());
      image_queue.pop();
    }
    char key = static_cast<char>(cv::waitKey(1));
    if (key == 27 || key == 'q' || key == 'Q') { // ESC/Q
      break;
    }
  }
  delete m_pSDK;
  return 0;

上述代码，用了 OpenCV 来显示图像。选中显示窗口时，按 ``ESC/Q`` 就会结束程序。

完整代码样例，请见 `_get_imu.cpp <https://github.com/indemind/IMSEE-SDK/blob/master/demo/get_imu.cpp>`_ 。

