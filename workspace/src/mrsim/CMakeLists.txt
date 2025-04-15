cmake_minimum_required(VERSION 3.16)
project(mrsim)

find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED)
find_package(jsoncpp REQUIRED)

set(CMAKE_CXX_STANDARD 17)

find_package(catkin REQUIRED COMPONENTS
    roscpp
    tf2
    tf2_ros
    tf2_eigen
    std_msgs
    pcl_ros
    pcl_conversions
    sensor_msgs
    geometry_msgs
    message_generation
)

catkin_package(
    INCLUDE_DIRS include
    LIBRARIES mrsim
    CATKIN_DEPENDS roscpp tf2 tf2_ros tf2_eigen std_msgs pcl_ros pcl_conversions sensor_msgs geometry_msgs
)

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
)

add_library(mrsim
    src/robot.cpp
    src/world.cpp
    src/lidar.cpp
)

target_link_libraries(mrsim
    ${catkin_LIBRARIES}
    ${OpenCV_LIBRARIES}
    Eigen3::Eigen
    jsoncpp_lib
)

add_executable(mrsim_node bin/mrsim_node.cpp)
target_link_libraries(mrsim_node
    mrsim
    ${catkin_LIBRARIES}
    ${PCL_LIBRARIES}
)

install(TARGETS mrsim_node
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

