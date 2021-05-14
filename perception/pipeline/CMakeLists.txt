cmake_minimum_required(VERSION 2.4.6)
project(pipeline)
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

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()
catkin_package()
find_package(Eigen3 REQUIRED)

include_directories(
  include
  ${EIGEN3_INCLUDE_DIR}
  #../dgcglobal/include
  #../driving_common/include
  ${catkin_INCLUDE_DIRS}
)
#common commands for building c++ executables and libraries
add_library(pipeline src/pipeline.cpp)
target_link_libraries(pipeline pthread)
#target_link_libraries(${PROJECT_NAME} another_library)

find_package(Boost REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})
 
catkin_add_gtest(test_pipeline test/test_pipeline.cpp)
#catkin_add_gtest_build_flags(test_pipeline)
target_link_libraries(test_pipeline pipeline ${Boost_LIBRARIES})
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})
