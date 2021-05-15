cmake_minimum_required(VERSION 2.4.6)
project(perception)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  message_generation
  driving_common
)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
# set(ROS_BUILD_TYPE Debug)

# set(CMAKE_CXX_FLAGS "-std=c++0x")
# set(ROS_COMPILE_FLAGS "-Wno-unknown-pragmas")
# rosbuild_init()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_message_files(
  FILES
  DynamicObstacle.msg
  PerceptionObstacles.msg
  ScanPoint.msg
  StaticObstaclePoint.msg
  VirtualScan.msg  
)
generate_messages(
  DEPENDENCIES
  std_msgs
  driving_common
)

catkin_package(
  CATKIN_DEPENDS message_runtime
)
#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)

# rosbuild_add_executable(perception src/main.cpp src/perception.cpp src/veloGroundFinder.cpp src/utils.cc src/kalman_multitracker.cpp)
# rosbuild_add_executable(perception src/line.cc src/ray_tracing.cc src/integration.cc src/scan.cc src/multibooster_support2.cpp)
# rosbuild_add_executable(perception src/tracker.cc src/box.cc src/segment.cpp src/obstacle.cpp src/veloClient.cpp)
# rosbuild_add_executable(perception src/tracked_obstacle.cpp src/radar.cc src/velodyne_rings.cpp src/pointCloudClient.cpp)
# cut from above command: src/laser_segment.cpp
