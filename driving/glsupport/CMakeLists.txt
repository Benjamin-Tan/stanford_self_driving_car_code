cmake_minimum_required(VERSION 2.4.6)
project(glsupport)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  driving_common
)
find_package(OpenCV REQUIRED)
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
catkin_package()

include_directories(
  include
  ../dgcglobal/include
  ../driving_common/include
  ../gls/include
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
)
#include($ENV{ROS_ROOT}/core/rosbuild/FindPkgConfig.cmake)
#pkg_check_modules(FTGL REQUIRED ftgl)
find_package(Freetype REQUIRED)
include_directories(${FREETYPE_INCLUDE_DIRS})
#include_directories(${QT_INCLUDE_DIR})
#link_directories(${FTGL_LIBRARY_DIRS})
add_library(textures src/textures.cpp)
target_link_libraries(textures ${OpenCV_LIBS})
add_library(gl_support src/gl_support.cc src/fontRenderer.cpp src/pointlist.cpp src/facelist.cpp)
add_dependencies(gl_support ${catkin_EXPORTED_TARGETS})
target_link_libraries(gl_support ${FREETYPE_LIBRARIES} ftgl glut GLU)
#target_link_libraries(gl_support ${FTGL_LIBRARIES})
