cmake_minimum_required(VERSION 3.0.2)
project(husky_mapping)

## Find catkin macros and libraries
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  rtabmap_ros
)

## Declare a catkin package
catkin_package(
  CATKIN_DEPENDS roscpp rospy std_msgs rtabmap_ros
)

## Specify additional locations of header files
include_directories(
  ${catkin_INCLUDE_DIRS}
)

## Install launch files
install(DIRECTORY launch
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)