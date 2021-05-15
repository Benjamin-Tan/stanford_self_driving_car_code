cmake_minimum_required (VERSION 2.4.6)
project(point_cloud_mapping)
## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  tf
)
# include ($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)
# set (ROS_BUILD_TYPE RelWithDebInfo)
# rosbuild_init ()
# rosbuild_add_boost_directories ()
#add_definitions(-Wall -O3 -DNDEBUG -march=pentium4 -mfpmath=sse -msse3)
#add_definitions (-Wall -O3 -DNDEBUG -mfpmath=sse -msse3)

#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
catkin_package()
find_package(Eigen3 REQUIRED)
include_directories(
  include
  ../../libraries/qpOASES-3.2.0/include
  ../multibooster/include
  ../pipeline/include
  ${EIGEN3_INCLUDE_DIR}
#   ${OpenCV_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)

add_executable (cloud_downsampler_node src/cloud_downsampler.cpp)
target_link_libraries (cloud_downsampler_node cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann)

add_executable (normal_estimation_node src/normal_estimation.cpp)
target_link_libraries (normal_estimation_node cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann)
# rosbuild_add_openmp_flags (normal_estimation_node)

add_executable (organized_normal_estimation_node src/organized_normal_estimation.cpp)
target_link_libraries (organized_normal_estimation_node cloud_geometry ${catkin_LIBRARIES} ann flann)
# rosbuild_add_openmp_flags (organized_normal_estimation_node)

add_library (normal_estimation_in_proc src/normal_estimation_in_proc.cpp)
# rosbuild_add_openmp_flags (normal_estimation_in_proc)
target_link_libraries(normal_estimation_in_proc cloud_geometry ${catkin_LIBRARIES} ann flann)

add_executable (planar_fit_node src/planar_fit.cpp)
target_link_libraries (planar_fit_node cloud_geometry cloud_kdtree sample_consensus ${catkin_LIBRARIES} ann flann)

add_executable (player_laser_log_to_msg src/player_laser_log_to_msg.cpp)
target_link_libraries (player_laser_log_to_msg cloud_geometry ${catkin_LIBRARIES} ann flann)

# ---[ Cloud Geometry library
  add_library (cloud_geometry
                       src/cloud_geometry/angles.cpp
                       src/cloud_geometry/areas.cpp
                       src/cloud_geometry/distances.cpp
                       src/cloud_geometry/intersections.cpp
                       src/cloud_geometry/nearest.cpp
                       src/cloud_geometry/point.cpp
                       src/cloud_geometry/projections.cpp
                       src/cloud_geometry/statistics.cpp
                       src/cloud_geometry/transforms.cpp
                      )
  target_link_libraries (cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann)
  # rosbuild_add_openmp_flags (cloud_geometry)
  catkin_add_gtest (test_geometry_statistics test/cloud_geometry/test_geometry_statistics.cpp)
  catkin_add_gtest (test_geometry_transform test/cloud_geometry/test_geometry_transform.cpp)
  target_link_libraries (test_geometry_transform cloud_geometry ${catkin_LIBRARIES})
  catkin_add_gtest (test_geometry_angles     test/cloud_geometry/test_geometry_angles.cpp)
  catkin_add_gtest (test_geometry_areas      test/cloud_geometry/test_geometry_areas.cpp)
  target_link_libraries (test_geometry_areas cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann)

# ---[ Cloud I/O library
  add_library (cloud_io
                       src/cloud_io/read.cpp
                       src/cloud_io/write.cpp
                       src/cloud_io/misc.cpp
                      )
  add_executable (cloudmsg_to_screen src/cloud_io/tools/cloudmsg_to_screen.cpp)
  target_link_libraries (cloudmsg_to_screen cloud_io ${catkin_LIBRARIES} ann flann)

  add_executable (pointcloud_to_pcd_node src/cloud_io/tools/pointcloud_to_pcd.cpp)
  target_link_libraries (pointcloud_to_pcd_node cloud_io cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann)

  add_executable (pcd_to_msg_node src/cloud_io/tools/pcd_to_msg.cpp)
  target_link_libraries (pcd_to_msg_node cloud_io cloud_geometry ${catkin_LIBRARIES} ann flann)

  add_executable (convert_pcd_ascii_binary src/cloud_io/tools/convert_pcd_ascii_binary.cpp)
  target_link_libraries (convert_pcd_ascii_binary cloud_io ${catkin_LIBRARIES} ann flann)


  catkin_add_gtest (test_cloud_io test/cloud_io/test_io.cpp)
  target_link_libraries (test_cloud_io cloud_io ${catkin_LIBRARIES} ann flann)

# ---[ Cloud Kd-Tree library
  add_library (cloud_kdtree
                       src/cloud_kdtree/kdtree_ann.cpp
                       src/cloud_kdtree/kdtree_flann.cpp
                      )

  catkin_add_gtest (test_cloud_kdtree test/cloud_kdtree/test_kdtree.cpp)
  target_link_libraries (test_cloud_kdtree cloud_kdtree ${catkin_LIBRARIES} ann flann)

# ---[ Cloud Octree library
  add_library (cloud_octree
                       src/cloud_octree/octree.cpp
                       src/cloud_octree/octree_node.cpp
                      )

# ---[ Sample-Consensus library
  add_library (sample_consensus
                       src/sample_consensus/sac.cpp
                       src/sample_consensus/sac_model.cpp
                       src/sample_consensus/lmeds.cpp
                       src/sample_consensus/ransac.cpp
                       src/sample_consensus/msac.cpp
                       src/sample_consensus/mlesac.cpp
                       src/sample_consensus/rransac.cpp
                       src/sample_consensus/rmsac.cpp
                       src/sample_consensus/sac_model_plane.cpp
                       src/sample_consensus/sac_model_normal_plane.cpp
                       src/sample_consensus/sac_model_oriented_plane.cpp
                       src/sample_consensus/sac_model_line.cpp
                       src/sample_consensus/sac_model_oriented_line.cpp
                       src/sample_consensus/sac_model_circle.cpp
                       src/sample_consensus/sac_model_sphere.cpp
                       src/sample_consensus/sac_model_cylinder.cpp
                       src/sample_consensus/sac_model_parallel_lines.cpp
                      )
  target_link_libraries(sample_consensus cloud_geometry ${catkin_LIBRARIES} ann flann cminpack)
  catkin_add_gtest (test_sample_consensus_line_fit test/sample_consensus/test_line_fit.cpp)
  catkin_add_gtest (test_sample_consensus_plane_fit test/sample_consensus/test_plane_fit.cpp)
  catkin_add_gtest (test_sample_consensus_circle_fit test/sample_consensus/test_circle_fit.cpp)
  catkin_add_gtest (test_sample_consensus_sphere_fit test/sample_consensus/test_sphere_fit.cpp)
  catkin_add_gtest (test_sample_consensus_cylinder_fit test/sample_consensus/test_cylinder_fit.cpp)

  target_link_libraries (test_sample_consensus_line_fit sample_consensus cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann cminpack)
  target_link_libraries (test_sample_consensus_plane_fit sample_consensus cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann cminpack)
  target_link_libraries (test_sample_consensus_circle_fit sample_consensus cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann cminpack)
  target_link_libraries (test_sample_consensus_sphere_fit sample_consensus cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann cminpack)
  target_link_libraries (test_sample_consensus_cylinder_fit sample_consensus cloud_geometry cloud_kdtree ${catkin_LIBRARIES} ann flann cminpack)

