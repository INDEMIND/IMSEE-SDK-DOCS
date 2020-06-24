.. _get_device_info:

获取设备信息
==============
通过SDK的``GetModuleParams``及``GetModuleInfo``分别可获取模组的参数信息及硬件信息。

参考代码片段：

.. code-block:: c++

  auto m_pSDK = new CIMRSDK();
  MRCONFIG config = {0};
  config.bSlam = false;
  config.imgResolution = IMG_640;
  config.imgFrequency = 50;
  config.imuFrequency = 1000;

  // m_pSDK->Init(config);
  indem::MoudleAllParam param = m_pSDK->GetModuleParams();
  indem::ModuleInfo info = m_pSDK->GetModuleInfo();

  std::cout << "Module info: " << std::endl;
  info.printInfo();
  std::cout << "Left param: " << std::endl;
  param._left_camera[RESOLUTION::RES_640X400].printInfo();
  std::cout << "Right param: " << std::endl;
  param._right_camera[RESOLUTION::RES_640X400].printInfo();
  std::cout << "Imu param: " << std::endl;
  param._imu.printInfo();
  return 0;
