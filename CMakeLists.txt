cmake_minimum_required(VERSION 3.2)

project(foo VERSION 0.1.0 LANGUAGES CXX)
include(GNUInstallDirs)

SET(VERSION_MAJOR "0")
SET(VERSION_MINOR "1")
SET(VERSION_PATCH "0")
SET(VERSION "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")
ADD_DEFINITIONS(-DVERSION="${VERSION}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")

add_library(libfoo SHARED
	src/foo.cpp
	)
add_library(Foo::libfoo ALIAS libfoo)
target_include_directories(libfoo 
	PUBLIC 
	  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
	  $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
	  )
target_compile_features(libfoo PUBLIC cxx_std_11)
set_target_properties(libfoo
	PROPERTIES
	  OUTPUT_NAME "foo"
	)

set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/Foo)
install(TARGETS
          libfoo
	EXPORT 
	  FooTargets
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	)

install(EXPORT FooTargets
	FILE FooTargets.cmake
	NAMESPACE Foo::
	DESTINATION ${INSTALL_CONFIGDIR}
	)

install(DIRECTORY include/
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
	FILES_MATCHING PATTERN "*.h"
	)

#####################
# ConfigVersion file
##
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/FooConfigVersion.cmake
	VERSION ${PROJECT_VERSION}
	COMPATIBILITY AnyNewerVersion
	)

configure_package_config_file(
	${CMAKE_CURRENT_LIST_DIR}/CMake/FooConfig.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/FooConfig.cmake
	INSTALL_DESTINATION ${INSTALL_CONFIGDIR}
	)

## Install all the helper files
install(
	FILES
	  ${CMAKE_CURRENT_BINARY_DIR}/FooConfig.cmake
	  ${CMAKE_CURRENT_BINARY_DIR}/FooConfigVersion.cmake
	DESTINATION ${INSTALL_CONFIGDIR}
	)

message("INSTALL_INCLUDE_DIR: ${CMAKE_INSTALL_INCLUDEDIR}")
