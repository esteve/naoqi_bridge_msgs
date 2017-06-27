cmake_minimum_required(VERSION 3.5)

project(naoqi_bridge_msgs)

if(NOT WIN32)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wall -Wextra -Wpedantic")
endif()

find_package(ament_cmake REQUIRED)
find_package(builtin_interfaces REQUIRED)
find_package(rosidl_default_generators REQUIRED)

set(message_deps
  builtin_interfaces
  actionlib_msgs
  sensor_msgs
  nav_msgs
  trajectory_msgs
  std_msgs
  geometry_msgs)

foreach(msg_dep ${message_deps})
  find_package(${msg_dep} REQUIRED)
endforeach()

# Find all the messages files
file(GLOB msg_files RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
      "${CMAKE_CURRENT_SOURCE_DIR}/msg/*.msg")

# Find all the service files
FILE(GLOB srv_files RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
      "${CMAKE_CURRENT_SOURCE_DIR}/srv/*.srv")

# Find all the action files
file(GLOB action_files RELATIVE "${CMAKE_CURRENT_SOURCE_DIR}"
      "${CMAKE_CURRENT_SOURCE_DIR}/action/*.action")

rosidl_generate_interfaces(${PROJECT_NAME}
  ${msg_files}
  ${srv_files}
#  ${action_files}
  DEPENDENCIES ${message_deps}
  ADD_LINTER_TESTS
)

ament_export_dependencies(rosidl_default_runtime)

ament_package()
