cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(example-usm)

set(LIBTORCH_PREFIX "${CMAKE_SOURCE_DIR}/libtorch/" CACHE PATH "Path to user installed Libtorch")

include(CMakeDependentOption)

cmake_dependent_option(LIBTORCH_INSTALL_DIR_AVAILABLE "Cannot find Intel Libtorch so specify your installation" ON "IPEX_INSTALL_PREFIX" OFF)

if(NOT LIBTORCH_INSTALL_DIR_AVAILABLE)
  MESSAGE(STATUS "This system does not already have PyTorch C++ SDK with intel extensions. Will look for required SDK in ${LIBTORCH_PREFIX}")
  # Add Prefix path to find config file for Torch
  list(APPEND CMAKE_SYSTEM_PREFIX_PATH "${LIBTORCH_PREFIX}/share/cmake/Torch/")
   # Add Prefix path to find config file for IPEX
  list(APPEND CMAKE_SYSTEM_PREFIX_PATH "${LIBTORCH_PREFIX}/share/cmake/IPEX/")
endif()

find_package(IPEX REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-dev")
set(target example-usm)
add_executable(${target} example-usm.cpp)
target_link_libraries(${target} ${TORCH_IPEX_LIBRARIES})
list(APPEND CMAKE_CXX_FLAGS "-fsycl")

set_property(TARGET ${target} PROPERTY CXX_STANDARD 17)
