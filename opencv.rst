=====================
OpenCV 的开发环境搭建
=====================


Mac \+ Xcode 的OpenCV环境搭建
-----------------------------

1. 安装 OpenCV
   
   使用brew工具安装OpenCV,默认已经安装了brew，如果没有安装参考网上的教程，很容易安装好。
   
   .. code-block:: shell
   
       brew install opencv 

   [注]网上有教程说，使用brew install opencv是安装的2.4的版本，``brew install opencv3``是安装3.x的最新版本。但是笔者
   当时使用的是``brew install opencv``安装的是3.4版本的。

2. 配置Xcode环境
   
   由于使用brew工具下载的项目都是放置在``/usr/local/Cellar``目录下得所以查看是否安装成功就是这个目录下的``opencv``目录，检查``lib``和``include``中是否有文件。

   新建项目选择``macOS``，``Command Line Tool``,``C++``项目,打开项目文件名配置环境变量

   + ``Build Phases``的配置中``Link Binary With Libraries``点开下拉框点击``+``从``/usr/local/Cellar/opencv/lib``添加\*.dylib库文件
   + ``Build Settings``中的``Search Paths``配置``Header Search Paths``添加为``/usr/local/include``并且将``Library Search Paths``中添为``/usr/local/lib``

3. 使用Opencv的例子代码测试是否运行成功  
