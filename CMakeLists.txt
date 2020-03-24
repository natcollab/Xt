#[===============================================================================[.rst
# Xt/CMakeLists.txt
#
# Xt library.
# ^^^^^^^^^^
#
# Imported targets in global scope:
#
#   Xt::Xt
#
#]===============================================================================]

#TODO: is the 'config' directory needed?

# Setup minimum required version of cmake, policy
# This command should be the first command in the project
cmake_minimum_required(VERSION 3.13...3.16)

# Setup Project name and version
# CMakeTest_VERSION and PROJECT_VERSION variables are set
project(Xt VERSION 1.0
  DESCRIPTION "Interface to Xt library"
  LANGUAGES CXX
  )

#=================================================================================

# Target: Xt
add_library(Xt INTERFACE)
add_library(Xt::Xt ALIAS Xt)

set(Xt_INCLUDE_INSTALL_DIR include/Xt)
set(Xt_LIB_INSTALL_DIR lib/Xt)
set(Xt_CMAKE_INSTALL_DIR lib/cmake/Xt)

target_include_directories(Xt
  INTERFACE  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
             $<INSTALL_INTERFACE:${Xt_INCLUDE_INSTALL_DIR}>
  )

target_link_libraries(Xt
  INTERFACE  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/libXtTraderApi.so>
             $<INSTALL_INTERFACE:${Xt_LIB_INSTALL_DIR}/libXtTraderApi.so>
  )

#=================================================================================

# Install : Xt
install(DIRECTORY include/
  DESTINATION ${Xt_INCLUDE_INSTALL_DIR}
  )

install(DIRECTORY lib/
  DESTINATION ${Xt_LIB_INSTALL_DIR}
  )

#=================================================================================

# Install export targets

install(TARGETS Xt EXPORT Xt_export)

# TODO: library path in export target file is relative path, which is not correct
install(EXPORT Xt_export
  NAMESPACE Xt::
  FILE XtTargets.cmake
  DESTINATION ${Xt_CMAKE_INSTALL_DIR}
  )

#=================================================================================

# Install package config and version files

include(CMakePackageConfigHelpers)

# Package version file
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/XtConfigVersion.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion
  )

# Package config file
configure_package_config_file(
  ${CMAKE_CURRENT_LIST_DIR}/cmake/XtConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/XtConfig.cmake
  INSTALL_DESTINATION ${Xt_CMAKE_INSTALL_DIR}
  PATH_VARS
    Xt_INCLUDE_INSTALL_DIR
    Xt_LIB_INSTALL_DIR
  )

# Install the config and version files
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/XtConfig.cmake
  ${CMAKE_CURRENT_BINARY_DIR}/XtConfigVersion.cmake
  DESTINATION ${Xt_CMAKE_INSTALL_DIR}
  )
