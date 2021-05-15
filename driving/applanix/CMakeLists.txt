cmake_minimum_required(VERSION 2.4.6)
project(applanix)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  message_generation
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
add_message_files(
  FILES
  ApplanixDMI.msg
  ApplanixGPS.msg
  ApplanixPose.msg
  ApplanixRMS.msg  
)
generate_messages(
  DEPENDENCIES
  std_msgs
)

catkin_package(
  CATKIN_DEPENDS message_runtime
)
find_library(DRIVING_COMMON driving_common)
find_library(DGCGLOBAL dgcglobal)
find_library(LLTRANSFORM lltransform)
find_library(Z z)
include_directories(
  include
  ../dgcglobal/include
  ../driving_common/include
  ../lltransform/include
  ${catkin_INCLUDE_DIRS}
)
if (DRIVING_COMMON AND LLTRANSFORM AND DGCGLOBAL AND Z)
  add_executable(applanix src/applanix.cc src/applanixcore.cc)
  add_dependencies(applanix ${${PROJECT_NAME}_EXPORTED_TARGETS} ${localize_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
  target_link_libraries(applanix ${Z} ${catkin_LIBRARIES} ${DRIVING_COMMON} ${DGCGLOBAL} ${LLTRANSFORM})
  #rosbuild_add_executable(applanix_logger src/applanix_logger.cc src/applanixcore.cc)
  #target_link_libraries(applanix_logger z)
  add_executable(fake_applanix src/fake_applanix.cc)
  add_dependencies(fake_applanix ${${PROJECT_NAME}_EXPORTED_TARGETS} ${localize_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
  target_link_libraries(fake_applanix ${Z} ${catkin_LIBRARIES} ${DRIVING_COMMON} ${DGCGLOBAL} ${LLTRANSFORM})
  #rosbuild_add_executable(applanix_integrate src/applanix_integrate.cc)
  #target_link_libraries(applanix_integrate z)
else()
  message(${PROJECT_NAME} " not build, missing dependencies")
endif()