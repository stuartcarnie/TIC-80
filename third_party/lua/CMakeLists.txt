cmake_minimum_required(VERSION 3.9)
project(lua C)

option(INCLUDE_LPEG "Include lpeg library" on)
option(ENABLE_LUA_COMPAT_5_2 "Enable LUA 5.2 Compatibility" on)

## options

if(ENABLE_LUA_COMPAT_5_2)
    add_definitions(-DLUA_COMPAT_5_2)
endif(ENABLE_LUA_COMPAT_5_2)


if (${CMAKE_HOST_SYSTEM_NAME} STREQUAL "Darwin")
    add_definitions(-DLUA_USE_MACOSX)
endif()

if (${CMAKE_HOST_SYSTEM_NAME} STREQUAL "Linux")
    add_definitions(-DLUA_USE_LINUX)
endif()

## SOURCES

set ( SRC_CORE src/lapi.c src/lcode.c src/lctype.c src/ldebug.c src/ldo.c src/ldump.c
        src/lfunc.c src/lgc.c src/llex.c src/lmem.c src/lobject.c src/lopcodes.c src/lparser.c
        src/lstate.c src/lstring.c src/ltable.c src/ltm.c src/lundump.c src/lvm.c src/lzio.c )

set ( SRC_LIB src/lauxlib.c src/lbaselib.c src/lbitlib.c src/lcorolib.c src/ldblib.c
        src/liolib.c src/lmathlib.c src/loslib.c src/lstrlib.c src/ltablib.c src/linit.c
        src/lutf8lib.c src/loadlib.c )

add_library(lua STATIC ${SRC_CORE} ${SRC_LIB})

if(INCLUDE_LPEG)
    target_link_libraries(lua lpeg)
endif(INCLUDE_LPEG)


target_include_directories(lua INTERFACE src)
