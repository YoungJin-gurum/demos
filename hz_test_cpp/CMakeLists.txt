cmake_minimum_required(VERSION 3.5)
project(hz_test_cpp)

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
find_package(example_interfaces REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_components REQUIRED)
find_package(action_tutorials_interfaces REQUIRED)
find_package(rclcpp_action REQUIRED)
find_package(rcutils)
find_package(rmw REQUIRED)
find_package(std_msgs REQUIRED)

include_directories(include)

add_library(hz_test SHARED
  src/hz_action_client.cpp
  src/hz_action_server.cpp)
rclcpp_components_register_node(hz_test PLUGIN "hz_test_cpp::HzClient" EXECUTABLE hz_client)
rclcpp_components_register_node(hz_test PLUGIN "hz_test_cpp::HzServer" EXECUTABLE hz_server)
target_compile_definitions(hz_test
  PRIVATE "HZ_TEST_CPP_BUILDING_DLL")
ament_target_dependencies(hz_test
  "action_tutorials_interfaces"
  "rclcpp"
  "rclcpp_action"
  "rclcpp_components")

install(TARGETS
  hz_test
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin)

ament_package()
