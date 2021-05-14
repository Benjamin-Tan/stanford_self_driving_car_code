cmake_minimum_required(VERSION 2.4.6)
project(matplotlib_interface)
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

#common commands for building c++ executables and libraries

#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_boost_directories()
find_package(Eigen3 REQUIRED)
include_directories(
  include
  ${EIGEN3_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)

find_package(PythonLibs REQUIRED)
add_library(matplotlib_interface matplotlib_interface.cpp)
add_executable(example example.cpp)
target_link_libraries(matplotlib_interface ${PYTHON_LIBRARIES})
target_link_libraries(example ${PYTHON_LIBRARIES})
target_link_libraries(example matplotlib_interface)