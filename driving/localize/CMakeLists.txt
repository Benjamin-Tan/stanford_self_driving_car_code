cmake_minimum_required(VERSION 2.4.6)
project(localize)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  message_generation
  tf
)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
find_package(OpenCV 3.4.5 REQUIRED)
# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

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
  LocalizePose.msg
)
generate_messages(
  DEPENDENCIES
  std_msgs
)

catkin_package(
  CATKIN_DEPENDS message_runtime
)
# Old DGC make system include and lib directories.
# @TODO: Remove once fully migrated to ROS
# include_directories(../../../include)
# link_directories(../../../lib)
include_directories(
  include
  ../dgcglobal/include
  ../driving_common/include
  ../lltransform/include
  ../../perception/dgcgrid/include
  ../../perception/velodyne/include
  ../asyncwriter/include
  ../gls/include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)
#common commands for building c++ executables and libraries
add_library(localize_terrainmap src/terrainmap.cpp)
target_link_libraries(localize_terrainmap lltransform)

add_executable(localize_velodyne src/localizeVelodyne.cpp src/veloClient.cpp src/laserMap.cpp)
target_link_libraries(localize_velodyne ${OpenCV_LIBS} ${catkin_LIBRARIES} localize_terrainmap z lltransform gls grid velocore velo_support)
#target_link_libraries(localize_velodyne z)

add_executable(fake_localize src/fake_localize.cpp)
target_link_libraries(fake_localize ${catkin_LIBRARIES} z driving_common lltransform)
#rosbuild_add_executable(map_velodyne src/map_velodyne.cpp)
#target_link_libraries(map_velodyne localize_terrainmap)

#rosbuild_add_executable(generate_rndf src/generate_rndf.cpp)
#target_link_libraries(generate_rndf localize_terrainmap)
#rosbuild_add_executable(velo_calibrate src/velo_calibrate.cpp)
#target_link_libraries(velo_calibrate lltransform)
#rosbuild_add_executable(test_localize_accuracy src/test_localize_accuracy.cpp)
#target_link_libraries(test_localize_accuracy lltransform)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
