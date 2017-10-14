cmake_minimum_required(VERSION 3.9)
project(liblpeg C)

option(ENABLE_LPEG_DEBUGGING "Enable LPEG Debugging" off)

## SOURCES

set( SRC_CORE lpcap.c lpcode.c lptree.c lpvm.c )

if(ENABLE_LPEG_DEBUGGING)
    add_definitions(-DLPEG_DEBUG)
    list(APPEND SRC_CORE lpprint.c)
endif()

include_directories(../lua/src)
add_library(lpeg STATIC ${SRC_CORE})

target_include_directories(lpeg PUBLIC .)
