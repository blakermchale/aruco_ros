cmake_minimum_required(VERSION 3.5)
project(aruco_ros)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(visualization_msgs REQUIRED)
find_package(cv_bridge REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(tf2_geometry_msgs REQUIRED)
find_package(OpenCV REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(aruco REQUIRED)
find_package(aruco_msgs REQUIRED)
find_package(image_transport REQUIRED)


include_directories(
  include
  ${OpenCV_INCLUDE_DIRS}
)

add_library(aruco_ros_utils src/aruco_ros_utils.cpp)
target_link_libraries(aruco_ros_utils aruco opencv_calib3d)

add_executable(single src/simple_single.cpp
                      src/aruco_ros_utils.cpp)
target_link_libraries(single aruco opencv_calib3d ${OpenCV_LIBRARIES})

# add_executable(double src/simple_double.cpp
#                       src/aruco_ros_utils.cpp)
# add_dependencies(double ${PROJECT_NAME}_gencfg)
# target_link_libraries(double ${catkin_LIBRARIES} ${OpenCV_LIBRARIES})

# add_executable(marker_publisher src/marker_publish.cpp
#                                 src/aruco_ros_utils.cpp)
# target_link_libraries(marker_publisher aruco opencv_calib3d ${OpenCV_LIBRARIES})

set(dependencies
  rclcpp
  sensor_msgs
  tf2
  tf2_ros
  tf2_geometry_msgs
  visualization_msgs
  geometry_msgs
  cv_bridge
  aruco_ros_interfaces
  image_transport
)

ament_target_dependencies(single ${dependencies})
# ament_target_dependencies(fractal_detector_node ${dependencies})

##########
## Test ##
##########
if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  ament_lint_auto_find_test_dependencies()
endif()

#############
## Install ##
#############
install(
  DIRECTORY launch config
  DESTINATION share/${PROJECT_NAME}
)

install(TARGETS single marker_publisher aruco_ros_utils
# multi
# double
  DESTINATION lib/${PROJECT_NAME}
)

ament_package()
