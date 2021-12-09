cmake_minimum_required(VERSION 3.5)

project(vehicle_msgs)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

find_package(ament_cmake_auto REQUIRED)
find_package(common)

ament_auto_find_build_dependencies()

include_directories(
    ${common_INCLUDE_DIRS}
    ${catkin_INCLUDE_DIRS}
)


rosidl_generate_interfaces(
  ${PROJECT_NAME}
  State.msg
  ControlSignal.msg
  Lane.msg
  LaneNet.msg
  VehicleParam.msg
  Vehicle.msg
  VehicleSet.msg
  OccupancyGridFloat.msg
  OccupancyGridUInt8.msg
  Circle.msg
  CircleObstacle.msg
  PolygonObstacle.msg
  ObstacleSet.msg
  FreeState.msg

  ArenaInfo.msg
  ArenaInfoDynamic.msg
  ArenaInfoStatic.msg
)

ament_auto_package()