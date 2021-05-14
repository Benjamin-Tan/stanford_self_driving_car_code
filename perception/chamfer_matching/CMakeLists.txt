cmake_minimum_required(VERSION 2.4.6)
project(chamfer_matching)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
#set(ROS_BUILD_TYPE RelWithDebInfo)
#rosbuild_init()
include_directories(
  include
  #../dgcglobal/include
  #../driving_common/include
  ${catkin_INCLUDE_DIRS}
)
add_library(chamfer_matching src/chamfer_matching.cpp)
