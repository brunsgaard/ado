cmake_minimum_required (VERSION 2.8.8)
project (ado)

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -pedantic")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# Set a default build type if none was specified (we are not using an IDE)
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    message(STATUS "Setting build type to 'Debug' as none was specified.")
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build." FORCE)
endif()

# Add src before libs to make use it don't use installed headers
include_directories(src)

# Google flags
find_library(GFLAGS_LIB gflags REQUIRED)
find_path(GFLAGS_INCLUDE gflags.h PATH_SUFFIXES gflags)
include_directories(${GFLAGS_INCLUDE})

# Google log
find_library(GLOG_LIB glog REQUIRED)
find_path(GLOG_INCLUDE logging.h PATH_SUFFIXES glog)
include_directories(${GLOG_INCLUDE})

# TBB (threadingbuildingblocks.org)
find_library(TBB_LIB tbb REQUIRED)
find_path(TBB_INCLUDE_DIR tbb.h PATH_SUFFIXES tbb REQUIRED)
include_directories(${TBB_INCLUDE_DIR})

# sparsehash
find_path(SPARSEHASH_INCLUDE_DIR sparse_hash_map PATH_SUFFIXES sparsehash REQUIRED)
include_directories(${SPARSEHASH_INCLUDE_DIR})


set(link_libs
  ${GLOG_LIB}
  ${GFLAGS_LIB}
  ${TBB_LIB}
)

add_subdirectory (src)

