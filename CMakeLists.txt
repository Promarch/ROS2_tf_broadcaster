cmake_minimum_required(VERSION 3.8)
project(tf_broadcaster)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(yaml-cpp REQUIRED)
find_package(Eigen3 REQUIRED)
include_directories("/usr/include/eigen3")
include_directories("/home/alex/mocap_ws/src/tf_broadcaster/include/")

add_executable(AddFrameSensor src/add_frame_sensor.cpp)
ament_target_dependencies(AddFrameSensor
  rclcpp
  tf2
  tf2_ros
  geometry_msgs
  yaml-cpp
  Eigen3
)

install(TARGETS
  AddFrameSensor
  DESTINATION lib/${PROJECT_NAME}
)

target_link_libraries(AddFrameSensor
  ${rclcpp_LIBRARIES}
  ${tf2_ros_LIBRARIES}
  yaml-cpp
)

# Install the config files
install(DIRECTORY
  config
  DESTINATION share/${PROJECT_NAME}
)

# Install the launch files
install(DIRECTORY
  launch
  DESTINATION share/${PROJECT_NAME}
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
