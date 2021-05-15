cmake_minimum_required(VERSION 2.4.6)
project(cluster_descriptors)
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
#set(ROS_BUILD_TYPE Debug)

#rosbuild_init()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()
catkin_package()

find_package(Eigen3 REQUIRED)
# find_package(OpenCV REQUIRED)
include_directories(
  include
  ../../libraries/qpOASES-3.2.0/include
  ../multibooster/include
  ../pipeline/include
  ${EIGEN3_INCLUDE_DIR}
#   ${OpenCV_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)
#common commands for building c++ executables and libraries
#rosbuild_add_library(cluster_descriptors cluster_descriptors.cpp cluster_descriptor_pipelines.cpp)
add_library(cluster_descriptors cluster_descriptors.cpp)
# target_link_libraries(cluster_descriptors ${OpenCV_LIBS})

# rosbuild_add_boost_directories()

catkin_add_gtest(test_cluster_descriptors test/test_cluster_descriptors.cpp)
# rosbuild_add_gtest_build_flags(test_cluster_descriptors)
target_link_libraries(test_cluster_descriptors cluster_descriptors)

#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

#rosbuild_add_link_flags(test_cluster_descriptors "-lprofiler")