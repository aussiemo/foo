cmake_minimum_required(VERSION 3.2)

project(foo)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")

set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR})

set(FOO_INSTALL_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/dinclude)
set(FOO_INSTALL_BIN_DIR ${PROJECT_SOURCE_DIR}/dbin)
set(FOO_INSTALL_LIB_DIR ${PROJECT_SOURCE_DIR}/dlib)

set(FOO_HEADERS_DIR ${PROJECT_SOURCE_DIR}/include)

include_directories(${FOO_INSTALL_INCLUDE_DIR})
include_directories(${FOO_HEADERS_DIR})

add_library(foo SHARED
	src/foo.cpp
	)
