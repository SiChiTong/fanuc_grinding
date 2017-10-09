cmake_minimum_required(VERSION 2.8.3)
project(fanuc_grinding_path_planning)
add_definitions(-std=c++11 -Wall -Wextra)

find_package(catkin REQUIRED COMPONENTS
  bezier_library
  genmsg
  geometry_msgs
  moveit_ros_planning_interface
  roscpp
  rviz_visual_tools
  std_msgs
  tf_conversions
  visualization_msgs
)

find_package(PCL 1.8.0 REQUIRED)

find_package(VTK 7.1.0 QUIET)
if (NOT VTK_FOUND)
  find_package(VTK 8.0.0 REQUIRED)
endif()
include(${VTK_USE_FILE})

add_service_files(FILES PathPlanningService.srv)
generate_messages(DEPENDENCIES geometry_msgs std_msgs)

catkin_package(CATKIN_DEPENDS
  bezier_library
  geometry_msgs
  message_runtime
  std_msgs
)

include_directories(
  ${VTK_USE_FILE}
  ${PCL_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)
add_definitions(
  ${PCL_DEFINITIONS}
  ${VTK_DEFINITIONS}
)

add_executable(
  ${PROJECT_NAME}
  src/path_planning.cpp
)
target_link_libraries(
  ${PROJECT_NAME}
  bezier_grinding_surfacing
  bezier_library
  ${PCL_LIBRARIES}
  ${VTK_LIBRARIES}
  ${catkin_LIBRARIES}
)
add_dependencies(
  ${PROJECT_NAME}
  ${PROJECT_NAME}_generate_messages_cpp
)
