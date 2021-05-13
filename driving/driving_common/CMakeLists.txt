cmake_minimum_required(VERSION 2.4.6)
project(driving_common)
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

add_message_files(
  FILES
    Actuator.msg
    CanStatus.msg
    ErrorString.msg
    EStopRequest.msg
    EStopStatus.msg
    GLSOverlay.msg
    HCIString.msg
    Heartbeat.msg
    LdlrsData.msg
    Pose.msg
    RadarSensorLRR3.msg
    RadarSensor.msg
    RadarTargetLRR3.msg
    RadarTarget.msg
    SimulatorGroundTruth.msg
    SimulatorTag.msg
    SimulatorVehiclePose.msg
    TrafficLightPose.msg
    TrafficLightState.msg
    TrafficLightStateRequest.msg
    TrafficLightStates.msg
    Trajectory2D.msg
    TrajectoryPoint2D.msg
    TurnSignal.msg
)
generate_messages(
  DEPENDENCIES
  std_msgs
)

catkin_package(
  CATKIN_DEPENDS message_runtime
)
#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()
include_directories(
  include
  ${catkin_INCLUDE_DIRS}
)
#common commands for building c++ executables and libraries
add_library(${PROJECT_NAME} src/comp_stdio.cpp src/lineBuffer.cpp src/scaledTime.cpp src/GlobalPose.cpp src/PoseQueue.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES})
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})
