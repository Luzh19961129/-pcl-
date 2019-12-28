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
代码编写
----
#include <iostream>
#include <pcl/io/pcd_io.h>
#include <pcl/point_types.h>
#include <pcl/filters/voxel_grid.h>
int
main (int argc, char** argv)
{
  pcl::PCLPointCloud2::Ptr cloud (new pcl::PCLPointCloud2 ());
  pcl::PCLPointCloud2::Ptr cloud_filtered (new pcl::PCLPointCloud2 ());
  // 填入点云数据
  pcl::PCDReader reader;
  // 存放点云数据的路径
  reader.read ("/home/luzh/Downloads/1.pcd", *cloud);
  std::cerr << "PointCloud before filtering: " << cloud->width * cloud->height 
       << " data points (" << pcl::getFieldsList (*cloud) << ").";
  // 创建滤波器对象
  pcl::VoxelGrid<pcl::PCLPointCloud2> sor;
  sor.setInputCloud (cloud);
  sor.setLeafSize (0.01f, 0.01f, 0.01f);// 单位：m
  sor.filter (*cloud_filtered);
  std::cerr << "PointCloud after filtering: " << cloud_filtered->width * cloud_filtered->height 
       << " data points (" << pcl::getFieldsList (*cloud_filtered) << ").";
  pcl::PCDWriter writer;
//自定义文件名输出
  writer.write ("table_scene_lms400_downsampled.pcd", *cloud_filtered, 
         Eigen::Vector4f::Zero (), Eigen::Quaternionf::Identity (), false);
  return (0);
}
     
