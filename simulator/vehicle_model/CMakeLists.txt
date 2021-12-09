cmake_minimum_required(VERSION 3.5)
project(vehicle_model)

# set(CMAKE_VERBOSE_MAKEFILE "true")
# set(CMAKE_BUILD_TYPE "Release")
# set(CMAKE_CXX_FLAGS "-std=c++11  -g")
# set(CMAKE_CXX_FLAGS_RELEASE "-O3 -Wall")
# set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -Wall")

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
find_package(ament_cmake_auto REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(common)

ament_auto_find_build_dependencies()

set(THIRD_PARTY_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/thirdparty)

include_directories(
    inc
    ${common_INCLUDE_DIRS}
    ${EIGEN3_INCLUDE_DIR}
    ${THIRD_PARTY_INCLUDE_DIRS}
)

ament_auto_add_library(hkust_pl_vehicle_model
    src/vehicle_model/vehicle_model.cc
    src/vehicle_model/ideal_steer_model.cc
    src/vehicle_model/idm_model.cc
    src/vehicle_model/ctx_idm_model.cc
    src/vehicle_model/controllers/pure_pursuit_controller.cc
    src/vehicle_model/controllers/pid_controller.cc
    src/vehicle_model/controllers/idm_velocity_controller.cc
    src/vehicle_model/controllers/ctx_idm_velocity_controller.cc
)
target_link_libraries(hkust_pl_vehicle_model
    hkust_pl_common
)

ament_auto_package()