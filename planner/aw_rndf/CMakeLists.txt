cmake_minimum_required(VERSION 2.4.6)
project(aw_rndf)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  driving_common
)
#include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)
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

#include($ENV{ROS_ROOT}/core/rosbuild/FindPkgConfig.cmake)
catkin_package()

  # ftgl + freetype
#pkg_check_modules(FTGL REQUIRED ftgl)
find_package(Freetype REQUIRED)
include_directories(${FREETYPE_INCLUDE_DIRS})
#link_directories(${FTGL_LIBRARY_DIRS})
find_package(CGAL REQUIRED)
find_package(Eigen3 REQUIRED)
include_directories(
  include
  ../../driving/dgcglobal/include
  ../../driving/driving_common/include
  ../../driving/lltransform/include
  ../../driving/glsupport/include
  ../../driving/gls/include
  ../aw_geometry/include
  ${EIGEN3_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)
add_library(aw_rndf src/aw_CheckpointList.cpp src/aw_Mission.cpp src/aw_SpeedLimitList.cpp src/aw_checkPoint.cpp src/aw_exit.cpp src/aw_lane.cpp src/aw_laneSegment.cpp src/aw_netElement.cpp src/aw_perimeter.cpp src/aw_perimeterPoint.cpp src/aw_roadNetwork.cpp src/aw_segment.cpp src/aw_spot.cpp src/aw_stop.cpp src/aw_SpeedLimit.cpp src/aw_StringTools.cpp src/aw_wayPoint.cpp src/aw_zone.cpp src/aw_laneQuadTree.cpp src/aw_roadNetworkSearch.cpp src/aw_intersection.cpp src/aw_trafficLight.cpp src/aw_crosswalk.cpp)
add_library(aw_rndf src/aw_roadNetworkGL.cpp src/aw_trafficLightGL.cpp)
target_link_libraries(aw_rndf CGAL::CGAL GL aw_geometry gl_support textures)
