cmake_minimum_required(VERSION 3.9)

# Search in the "cmake" directory for additional CMake modules.
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

include(ucm)

project(TIC-80)

######################################################################
# Directories

set(GIFLIB_DIR ${CMAKE_SOURCE_DIR}/third_party/giflib)

# Search in the "cmake" directory for additional CMake modules.
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}/lib)
set(ASSETS_PATH "${CMAKE_SOURCE_DIR}/bin/assets")

file(MAKE_DIRECTORY ${ASSETS_PATH})

######################################################################
# Common definitions to compile all sources (app code and third party)

# giflib
set(GIF_LIBRARIES giflib)
set(GIF_INCLUDE_DIRS ${GIFLIB_DIR}/lib)
include_directories(${GIF_INCLUDE_DIRS})

# lualib
set(LIBLUA_LIBRARIES lua)


#include_directories(include/lua)
include_directories(include/zlib)
#include_directories(include/gif)
include_directories(include/sdl2)
include_directories(include/tic80)

# Third parties
add_subdirectory(third_party)
add_subdirectory(tools/bin2txt)
add_subdirectory(src)
add_dependencies(tic bin2txt)

#get_cmake_property(_variableNames VARIABLES)
#foreach (_variableName ${_variableNames})
#    message(STATUS "${_variableName}=${${_variableName}}")
#endforeach()