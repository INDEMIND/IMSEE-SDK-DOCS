.. _sdk_install_windows_src:

Windows 源码安装
=====================

.. only:: html

  +-----------------+
  | Windows 10      |
  +=================+
  | |build_passing| |
  +-----------------+

  .. |build_passing| image:: https://img.shields.io/badge/build-passing-brightgreen.svg?style=flat

.. only:: latex

  +-----------------+
  | Windows 10      |
  +=================+
  | ✓               |
  +-----------------+


软件准备
-----------
* CMake(3.0以上)(需要支持vs2019)
* Visual Studio 2019
* opencv3.４.３
* IMSEE-SDK

配置准备
-----------
* 将CMake安装路径下bin目录路径添加到系统环境变量"PATH"中。例如："C:\Program Files\CMake\bin"
* 设置".sln"文件的默认打开方式为"Microsoft Visual Studio 2019"。
* 右键".sln"文件，选择"打开方式" -> "选择其他应用"，弹出打开应用小窗口。
* 选中"Visual Studio 2019"，勾选"始终使用此应用打开.sln文件"，点击"确定"按钮。

编译OpenCV
-----------
* 双击"opencv-3.3.1-vc14.exe"，解压文件到指定目录下。例如：解压目录为："D:\opencv331"。
* 启动CMake，在"Where is the source code"中输入opencv源码路径。例如：源码路径为："D:\opencv331\opencv\sources"。
* 在"Where to build the binaries"中输入"opencv构建目录"(即，"sources"所在路径)。例如：构建目录为："D:\opencv331\opencv\build-win10-x64-vs19"。
* 点击"Configure"按钮，选择编译工具为"Visual Studio 16 2019"，选择编译目标平台为"x64"。点击"Finish"按钮，进行第一次配置。
* 第一次配置"Configuring done"后，在配置窗口中，选中"BUILD_opencv_world"，取消选中"BUILD_DOCS"、"BUILD_EXAMPLES"、"BUILD_TESTS"(取消选中可减少opencv编译时间)。再点击"Configure"按钮，进行第二次编译配置。第二次配置"Configuring done"后，点击"Generate"按钮，进行win项目生成.。"Generating done"后，点击"Open Project"按钮(或用vs2019打开构建目录下的OpenCV.sln)。
* 在vs2019窗口，选择编译目标平台为"Release"，点击"生成"->"生成解决方案"，开始编译。编译成功后，生成的文件在"opencv构建目录\bin\Release"下。
* 将"opencv构建目录\bin\Release"添加到环境变量"PATH"。例如："D:\...\opencv331\opencv\build-win10-x64-vs19\bin\Release"。
* 新建系统变量"OpenCV_DIR"，值为"opencv构建目录"。例如："D:\...\opencv331\opencv\build-win10-x64-vs19"。

编译Demo
-----------
* 双击文件"IMSEE-SDK\demo\build-demo.bat"，会自动打开"cmake-gui.exe"。注意：不要手动关闭"build-demo.bat"，它会自动关闭。
* 在CMake窗口，点击"Configure"按钮，选择编译工具为"Visual Studio 16 2019"，选择编译目标平台为"x64"。点击"Finish"按钮，此时CMake进行编译配置。
* "Configuring done"后，点击"Generate"按钮，进行win项目生成。
* "Generating done"后，关闭CMake窗口。脚本自动用vs2019打开"build/indemind_demos.sln"。注意：vs2019打开后，"build-demo.bat"会自动关闭
* 在vs2019中选择"Release"版本，点击"生成"->"生成解决方案"，开始编译demo。生成的文件在"IMSEE-SDK\demo\output\bin"下。

运行Demo
-----------
* 转到目录"IMSEE-SDK\demo\output\bin"，运行目录下的各个demo。

.. tip::
  * 编译目标平台要指定为"x64"。
  * 使用"Visual Studio 2015"或其他编译工具时，与上面步骤相同。