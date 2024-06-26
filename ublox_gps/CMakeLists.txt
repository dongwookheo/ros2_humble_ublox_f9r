cmake_minimum_required(VERSION 3.5)
project(ublox_gps)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

find_package(ament_cmake_ros REQUIRED)
# find_package(asio REQUIRED)
find_package(Boost 1.85.0 HINTS /home/hdwook3918/thirdparty/boost/install/lib/cmake COMPONENTS system thread regex)
find_package(diagnostic_msgs REQUIRED)
find_package(diagnostic_updater REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(rcl_interfaces REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(mavros_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(ublox_msgs REQUIRED)
find_package(ublox_serialization REQUIRED)

include_directories(include)

# build node
add_library(ublox_gps
  src/adr_udr_product.cpp
  src/gnss.cpp
  src/gps.cpp
  src/mkgmtime.c
  src/node.cpp
  src/raw_data_pa.cpp
  src/raw_data_product.cpp
  src/ublox_firmware.cpp
  src/ublox_firmware8.cpp
  src/ublox_firmware9.cpp)
ament_target_dependencies(ublox_gps
  "Boost"
  "diagnostic_msgs"
  "diagnostic_updater"
  "geometry_msgs"
  "rcl_interfaces"
  "rclcpp"
  "rclcpp_components"
  "sensor_msgs"
  "mavros_msgs"
  "std_msgs"
  "tf2"
  "ublox_msgs"
  "ublox_serialization"
)

target_include_directories(ublox_gps PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
  ${ublox_msgs_INCLUDE_DIRS}
  ${ublox_serialization_INCLUDE_DIRS}
)

install(TARGETS ublox_gps
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

add_executable(ublox_gps_node src/node_main.cpp)
ament_target_dependencies(ublox_gps_node
  "rclcpp"
)
target_link_libraries(ublox_gps_node ublox_gps)

# build logger node
add_executable(ublox_logger_node src/logger_node_pa.cpp src/raw_data_pa.cpp)
set_target_properties(ublox_logger_node PROPERTIES OUTPUT_NAME ublox_logger)
ament_target_dependencies(ublox_logger_node
  "rclcpp"
  "std_msgs"
)

install(TARGETS
  ublox_gps_node ublox_logger_node
  DESTINATION lib/${PROJECT_NAME}
)

rclcpp_components_register_nodes(ublox_gps
  "ublox_node::UbloxNode")

install(DIRECTORY include/
  DESTINATION include
)

install(
  DIRECTORY launch config
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
