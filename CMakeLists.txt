cmake_minimum_required(VERSION 3.5)
project(ros2_logging_fmt)

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
find_package(rclcpp_lifecycle REQUIRED)
find_package(fmt REQUIRED)


add_executable(test_logging src/test_logging.cpp)

target_include_directories(test_logging
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

ament_target_dependencies(test_logging rclcpp rclcpp_lifecycle fmt)

add_library(ros2_logging_fmt SHARED
  src/empty.cpp
)
ament_target_dependencies(ros2_logging_fmt
  rclcpp rclcpp_lifecycle fmt
)
target_include_directories(ros2_logging_fmt
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

ament_export_dependencies(rclcpp fmt)

install(
  DIRECTORY include/
  DESTINATION include
)

install(TARGETS test_logging ros2_logging_fmt
  EXPORT export_${PROJECT_NAME}
  RUNTIME DESTINATION lib/${PROJECT_NAME})

ament_export_include_directories(include)
ament_export_targets(export_${PROJECT_NAME} HAS_LIBRARY_TARGET)
ament_package()

