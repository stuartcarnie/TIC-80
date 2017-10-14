cmake_minimum_required(VERSION 3.9)
project(bin2txt C)

set( SRC_CORE bin2txt.c )

add_executable(bin2txt ${SRC_CORE})
target_link_libraries(bin2txt z)
