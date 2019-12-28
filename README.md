基于pcl的点云下采样
===
前期准备工作
----
配置Linux下的PCL开发环境.因为ubuntu库中自带PCL1.7，但总莫名其妙出问题，所以选择源码编译安装<br>
首先安装各种依赖库，参考https://github.com/huningxin/install_pcl_deps/blob/master/install_pcl_deps.sh。<br>
下载源码git clone https://github.com/PointCloudLibrary/pcl.git，这是最新版的PCL库（pcl1.9）<br>
解压和编译源码<br>
编写CMakeLists.txt指定编译规则:<br>
cmake_minimum_required(VERSION 2.4.6)<br>
project(test_pcl)<br>
if(COMMAND cmake_policy)<br>
     cmake_policy(SET CMP0003 NEW)<br>
endif(COMMAND cmake_policy)<br>
find_package(PCL 1.9 REQUIRED)<br>
include_directories(${PCL_INCLUDE_DIRS})<br>
add_executable(${PROJECT_NAME} main.cpp)<br>
target_link_libraries (${PROJECT_NAME}<br>
    ${PCL_LIBRARIES}<br>
 )<br>
 至此开发环境搭建完成。<br>
