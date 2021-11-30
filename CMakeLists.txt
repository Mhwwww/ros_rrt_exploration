cmake_minimum_required(VERSION 3.0.2)
project(ros_autonomous_slam)

find_package(catkin REQUIRED COMPONENTS
  geometry_msgs
  nav_msgs
  roscpp
  rospy
  std_msgs
  tf
  visualization_msgs
  message_generation
)

## Generate messages in the 'msg' folder
add_message_files(
  FILES
  PointArray.msg
)

## Generate services in the 'srv' folder
generate_messages(
  DEPENDENCIES
  std_msgs
  geometry_msgs
)

catkin_package( 

CATKIN_DEPENDS message_runtime
)

include_directories(include ${catkin_INCLUDE_DIRS})

add_executable(global_rrt_detector src/global_rrt_detector.cpp src/functions.cpp src/mtrand.cpp)
target_link_libraries(global_rrt_detector ${catkin_LIBRARIES})

add_executable(local_rrt_detector src/local_rrt_detector.cpp src/functions.cpp src/mtrand.cpp)
target_link_libraries(local_rrt_detector ${catkin_LIBRARIES})



