cmake_minimum_required(VERSION 3.8)
project(gazebocontrolsim)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(ament_cmake_python REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)
find_package(std_msgs REQUIRED)
find_package(control_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(trajectory_msgs REQUIRED)

# Include directories
include_directories(include)

# TODO: C++ nodes need to be migrated from ROS1 to ROS2
# The pose_control and controller_server nodes use ROS1 APIs (ros/ros.h)
# They are temporarily disabled until migration is complete
# For now, use MoveIt2's built-in controllers and planning capabilities

# # Build C++ executables
# add_executable(pose_control src/pose_control.cpp)
# ament_target_dependencies(pose_control
#   rclcpp
#   std_msgs
#   control_msgs
#   sensor_msgs
#   trajectory_msgs
# )
# 
# add_executable(controller_server src/controller_server.cpp)
# ament_target_dependencies(controller_server
#   rclcpp
#   std_msgs
#   control_msgs
#   sensor_msgs
#   trajectory_msgs
# )
# 
# # Install C++ executables
# install(TARGETS
#   pose_control
#   controller_server
#   DESTINATION lib/${PROJECT_NAME}
# )

# Install directories
install(DIRECTORY
  launch
  config
  urdf
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
