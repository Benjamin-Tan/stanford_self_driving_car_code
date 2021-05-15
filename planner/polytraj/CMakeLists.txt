cmake_minimum_required(VERSION 2.4.6)
project(polytraj)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  driving_common
)
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

include_directories(
  include
  ../../driving/driving_common/include
  ../../driving/dgcglobal/include
  ${catkin_INCLUDE_DIRS}
)
add_library(poly_traj src/keepVelocityTrajSet.cpp src/keep_lane_traj_set.cpp src/keepLaneAndVelocitySet2D.cpp src/keepLaneAndTrackPointSet2D.cpp src/trackPointTrajSet.cpp src/polyTraj.cpp src/polyTraj2D.cpp src/rootTraj.cpp src/horner.cpp)
add_dependencies(poly_traj ${catkin_EXPORTED_TARGETS})