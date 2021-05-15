cmake_minimum_required(VERSION 2.4.6)
project(aw_topology)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  driving_common
  perception
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
#pkg_check_modules(FTGL REQUIRED ftgl)
catkin_package()

find_package(Freetype REQUIRED)
include_directories(${FREETYPE_INCLUDE_DIRS})
#link_directories(${FTGL_LIBRARY_DIRS})
find_package(Eigen3 REQUIRED)
include_directories(
  include
  ../../driving/dgcglobal/include
  ../../driving/driving_common/include
  ../aw_rndf/include
  ../aw_geometry/include
  ../curvesmoothing/include
  ../polytraj/include
  ../../driving/lltransform/include
  ../../driving/glsupport/include
  ../../driving/gls/include
  ${EIGEN3_INCLUDE_DIR}
  ${catkin_INCLUDE_DIRS}
)
add_library(${PROJECT_NAME} src/aw_BlockadeManager.cpp src/aw_graph_tools.cpp src/aw_RndfGraphBuilder.cpp)
add_library(${PROJECT_NAME} src/aw_RndfIntersection.cpp src/aw_Topology.cpp)
add_library(${PROJECT_NAME} src/aw_match_to_graph.cpp src/aw_RndfGraph.cpp src/aw_RndfVertex.cpp)
add_library(${PROJECT_NAME} src/aw_Vehicle.cpp src/aw_Graph.cpp src/aw_RndfEdge.cpp src/aw_RndfGraphSearch.cpp)
add_library(${PROJECT_NAME} src/aw_Route.cpp src/aw_VehicleManager.cpp)
add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME} ${FREETYPE_LIBRARIES} ${catkin_LIBRARIES})
#target_link_libraries(${PROJECT_NAME} ${FTGL_LIBRARIES})

