cmake_minimum_required(VERSION 3.5)
project(common)

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

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
# find dependencies
find_package(ament_cmake_auto REQUIRED)
find_package(Protobuf REQUIRED)
set(THIRD_PARTY_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/thirdparty)
set(THIRD_PARTY_INCLUDE_DIRS  ${THIRD_PARTY_INCLUDE_DIRS} ${PROTOBUF_INCLUDE_DIR})
set(OOQP_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/thirdparty/ooqp/include/ooqp)
set(PROTOBUF_GENERATE_CPP_APPEND_PATH TRUE PARENT_SCOPE)

find_package(Eigen3 REQUIRED)
# find_package(Glog REQUIRED)
find_package(OpenCV REQUIRED)

find_package(OOQP REQUIRED)
if(NOT OOQP_FOUND)
    message(FATAL_ERROR "OOQP not found.")
endif()
ament_auto_find_build_dependencies()
list(APPEND ${PROJECT_NAME}_CONFIG_EXTRAS
  "common-extras.cmake"
)


set(THIRD_PARTY_LIBRARIES
    dw
    ${GLOG_LIBRARIES}
    ${PROTOBUF_LIBRARY}
)

include_directories(
    inc
    ${EIGEN3_INCLUDE_DIR}
    ${THIRD_PARTY_INCLUDE_DIRS}
    ${OOQP_INCLUDE_DIRS}
)

ament_auto_add_library(hkust_pl_ooqp_itf
    src/common/solver/ooqp_interface.cc
    src/common/solver/qp_solver.cc
)
target_link_libraries(hkust_pl_ooqp_itf
    ${OOQP_LIBRARIES}
)
set(COMMON_SRC
    src/common/basics/colormap.cc
    src/common/basics/semantics.cc
    src/common/basics/shapes.cc
    src/common/basics/tool_func.cc
    src/common/math/calculations.cc
    src/common/rss/rss_checker.cc
    src/common/idm/intelligent_driver_model.cc
    src/common/mobil/mobil_model.cc
    src/common/mobil/mobil_behavior_prediction.cc
    src/common/spline/lookup_table.cc
    src/common/spline/spline_generator.cc
    src/common/lane/lane.cc
    src/common/lane/lane_generator.cc
    src/common/primitive/frenet_primitive.cc
    src/common/state/state_transformer.cc
    src/common/state/free_state.cc
    src/common/circle_arc/circle_arc.cc
    src/common/circle_arc/circle_arc_branch.cc
)

ament_auto_add_library(hkust_pl_common
    ${COMMON_SRC}
)
target_link_libraries(hkust_pl_common
    ${THIRD_PARTY_LIBRARIES}
    opencv_core
    opencv_highgui
    hkust_pl_ooqp_itf
)

install(
  DIRECTORY cmake
  DESTINATION share/${PROJECT_NAME})

ament_auto_package(CONFIG_EXTRAS "${PROJECT_NAME}-extras.cmake")