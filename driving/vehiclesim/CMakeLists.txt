cmake_minimum_required(VERSION 2.4.6)
project(vehiclesim)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  applanix
  tf
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
catkin_package()
include_directories(
	include
    ../dgcglobal/include
    ../driving_common/include
    ../lltransform/include
    ../../planner/aw_rndf/include
	${catkin_INCLUDE_DIRS}
)

add_library(vehicle src/vehicle.cpp)
add_dependencies(vehicle ${catkin_EXPORTED_TARGETS})
target_link_libraries(vehicle driving_common dgcglobal lltransform)

add_executable(vehiclesim src/vehiclesim.cpp)
add_dependencies(vehiclesim ${catkin_EXPORTED_TARGETS})
target_link_libraries(vehiclesim vehicle ${catkin_LIBRARIES})

add_executable(multisim src/multisim.cpp src/lasersim.cpp src/trafficlights.cpp src/crosswalks.cpp)
add_dependencies(multisim ${catkin_EXPORTED_TARGETS})
target_link_libraries(multisim vehicle ${catkin_LIBRARIES} aw_rndf)
#rosbuild_add_executable(spawn_software src/spawn_software.cpp)
#rosbuild_add_executable(spawn_centrals src/spawn_centrals.cpp)
#rosbuild_add_executable(setup_simulation src/setup_simulation.cc)
#rosbuild_add_executable(extract_start_state src/extract_start_state.cc)
#rosbuild_add_executable(place_cars src/place_cars.cpp src/rndf_lookup.cpp)
