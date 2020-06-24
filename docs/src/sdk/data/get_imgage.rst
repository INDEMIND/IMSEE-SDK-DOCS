.. _get_image:

获取原始图像
==============

获取图像前，需要初始化SDK,然后直接通过回调获取。

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
  double last_img_time = -1.0;
  m_pSDK->RegistImgCallback([&last_img_time, &img_count, &image_queue](
                                double time, cv::Mat left, cv::Mat right) {
    if (!left.empty() && !right.empty()) {
      cv::Mat img;
      cv::hconcat(left, right, img);
      if (last_img_time >= 0) {
        std::ostringstream ss;
        double fps = 1.0 / (time - last_img_time);
        ss << std::fixed << "time stamp: " << std::setprecision(2) << time
           << ", fps: " << fps;
        cv::putText(img, ss.str(), cv::Point(25, 25), FONT_FACE, FONT_SCALE,
                    FONT_COLOR, THICKNESS);
      }
      last_img_time = time;
      image_queue.push(img);
      ++img_count;
    }
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

完整代码样例，请见 `_get_image.cpp <https://github.com/indemind/IMSEE-SDK/blob/master/demo/get_image.cpp>`_ 。

