cmake_minimum_required(VERSION 3.8)
project(mutifingerhand_moveit_demo)

# Default to C++17
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Find dependencies
find_package(ament_cmake REQUIRED)
find_package(moveit_core REQUIRED)
find_package(moveit_ros_planning REQUIRED)
find_package(moveit_ros_planning_interface REQUIRED)
find_package(moveit_visual_tools REQUIRED)
find_package(rviz_visual_tools REQUIRED)
find_package(pluginlib REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(tf2_eigen REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)

# TODO: C++ demo nodes need to be migrated from ROS1 to ROS2
# The move_group_interface_demo uses ROS1 APIs
# It is temporarily disabled until migration is complete
# For now, use MoveIt2's Python API or RViz plugin for demos

# include_directories(include)
# 
# add_executable(move_group_interface_demo src/move_group_interface_demo.cpp)
# ament_target_dependencies(move_group_interface_demo
#   moveit_core
#   moveit_ros_planning_interface
#   moveit_visual_tools
#   rviz_visual_tools
#   rclcpp
#   rclcpp_action
#   tf2_ros
#   tf2_eigen
#   tf2_geometry_msgs
# )
# 
# install(TARGETS
#   move_group_interface_demo
#   DESTINATION lib/${PROJECT_NAME}
# )

# Install launch files
install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}
)

ament_package()

#install(DIRECTORY launch DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
