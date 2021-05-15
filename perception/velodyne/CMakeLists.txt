cmake_minimum_required(VERSION 2.4.6)
project(velodyne)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  geometry_msgs
  message_generation
  pcl_ros
  tf
  driving_common
)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE Debug)

#set(CMAKE_CXX_FLAGS "-std=c++0x")

#rosbuild_init()

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()

add_message_files(
  FILES
  Block.msg
  BlockRaw.msg
  Packet.msg
  Projected.msg
  Return.msg
  ScanPoint.msg
  Stat.msg
)

generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
)

catkin_package(
  CATKIN_DEPENDS message_runtime
)

include_directories(
  include
  ../../driving/dgcglobal/include
  ../../driving/driving_common/include
  ../../driving/asyncwriter/include
  #../../perception/grid/include
  ${catkin_INCLUDE_DIRS}
)
find_library(DRIVING_COMMON driving_common)
find_library(ASYNC_WRITER async_writer)

if (DRIVING_COMMON AND ASYNC_WRITER)
  add_library(velocore src/velodyne.cpp src/velodyneConfig.cpp)
  add_library(velo_support src/velo_support.cpp)
  add_library(velodyne_nodelets src/Points.cpp src/Driver.cpp src/Project3D.cpp src/velodyneConfig.cpp)
  add_executable(velodyne src/velodyne_main.cpp)

  add_dependencies(velocore ${catkin_EXPORTED_TARGETS})
  add_dependencies(velo_support ${catkin_EXPORTED_TARGETS})
  add_dependencies(velodyne_nodelets ${catkin_EXPORTED_TARGETS})
  add_dependencies(velodyne ${catkin_EXPORTED_TARGETS})

  target_link_libraries(velocore velocore z ${catkin_LIBRARIES} ${DRIVING_COMMON} ${ASYNC_WRITER})
  target_link_libraries(velo_support velocore z ${catkin_LIBRARIES} ${DRIVING_COMMON} ${ASYNC_WRITER})
  target_link_libraries(velodyne_nodelets velocore z ${catkin_LIBRARIES} ${DRIVING_COMMON} ${ASYNC_WRITER})
  target_link_libraries(velodyne velocore z ${catkin_LIBRARIES} ${DRIVING_COMMON} ${ASYNC_WRITER})
endif()
#rosbuild_add_executable(velodyne_logger src/velodyne_logger.cpp)
#target_link_libraries(velodyne_logger velodyne_interface)
#rosbuild_add_executable(vlf_index src/vlf_index.cpp)
#target_link_libraries(vlf_index velo_support velocore z)
#rosbuild_add_executable(vlf_cut src/vlf_cut.cpp)
#target_link_libraries(vlf_cut velocore z )
#rosbuild_add_executable(velodyne_dump src/velodyne_dump.cpp)
#target_link_libraries(velodyne_dump velocore)
#rosbuild_add_executable(velodyne_stat src/velodyne_stat.cc)
#target_link_libraries(velodyne_stat velocore z)
