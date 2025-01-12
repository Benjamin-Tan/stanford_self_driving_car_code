cmake_minimum_required(VERSION 2.4.6)
project(linear_kalman_filter)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

#rosbuild_init()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
catkin_package()

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()
find_package(Eigen3 REQUIRED)

include_directories(
  include
  #../../driving/dgcglobal/include
  #../../driving/driving_common/include
  ../../libraries/matplotlib_interface/include
  ${EIGEN3_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)

find_library(MATPLOTLIB_INTERFACE matplotlib_interface)

if(MATPLOTLIB_INTERFACE)
  #common commands for building c++ executables and libraries
  add_library(${PROJECT_NAME} src/linear_kalman_filter.cpp)
  #target_link_libraries(${PROJECT_NAME} another_library)
  #rosbuild_add_boost_directories()
  #rosbuild_link_boost(${PROJECT_NAME} thread)
  add_executable(example src/example.cpp)
  target_link_libraries(example ${PROJECT_NAME} ${MATPLOTLIB_INTERFACE})
else()
  message(${PROJECT_NAME} " not build, missing dependencies")
endif()