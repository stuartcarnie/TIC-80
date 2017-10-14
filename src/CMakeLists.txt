cmake_minimum_required(VERSION 3.9)

set(CMAKE_C_STANDARD 99)

if (APPLE)
    link_directories(../lib/macos)
    find_library(COREAUDIO_LIBRARY CoreAudio)
    find_library(AUDIOTOOLBOX_LIBRARY AudioToolbox)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(COREVIDEO_LIBRARY CoreVideo)
    find_library(FORCEFEEDBACK_LIBRARY ForceFeedback)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(CARBON_LIBRARY Carbon)
    set(PLATFORM_LIBS
            ${COREAUDIO_LIBRARY} ${AUDIOTOOLBOX_LIBRARY} ${COCOA_LIBRARY}
            ${COREVIDEO_LIBRARY} ${FORCEFEEDBACK_LIBRARY} ${IOKIT_LIBRARY}
            ${CARBON_LIBRARY}
            objc m iconv)
    set(PLATFORM_SOURCES ext/file_dialog.m)
    add_definitions(-D_THREAD_SAFE -D_GNU_SOURCE)
    ucm_add_flags(-Wno-typedef-redefinition)
endif (APPLE)

set(SOURCES
        studio.c
        console.c
        run.c
        ext/file_dialog.c
        ext/md5.c
        ext/gif.c
        ext/net/SDLnet.c
        ext/net/SDLnetTCP.c
        ext/net/SDLnetselect.c
        fs.c
        tools.c
        start.c
        sprite.c
        map.c
        sfx.c
        music.c
        history.c
        world.c
        config.c
        keymap.c
        code.c
        dialog.c
        menu.c
        net.c
        surf.c
        ticapi.h
        )

set(TIC80_H ../include/tic80/tic80_types.h ../include/tic80/tic80.h ../include/tic80/tic80_config.h tic.h ticapi.h machine.h defines.h)

set(SOURCES_EXT html.c)

set(TIC80_SRC
        tic80.c
        tic.c
        ext/blip_buf.c
        jsapi.c
        luaapi.c
        ext/duktape/duktape.c)

add_executable(tic
        ${TIC80_SRC}
        ${SOURCES}
        ${SOURCES_EXT}
        ${PLATFORM_SOURCES}
        ${TIC80_H}
        )
target_link_libraries(tic SDL2 ${GIF_LIBRARIES} ${LIBLUA_LIBRARIES} z ${PLATFORM_LIBS})

########################
## Generate .dat files

file(GLOB demo_data_files RELATIVE ${CMAKE_SOURCE_DIR}/demos "${CMAKE_SOURCE_DIR}/demos/*.tic")
foreach(fn ${demo_data_files})
    add_custom_command(OUTPUT ${ASSETS_PATH}/${fn}.dat
            COMMAND ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/bin2txt ${fn} ${ASSETS_PATH}/${fn}.dat -z
            DEPENDS ${CMAKE_SOURCE_DIR}/demos/${fn}
            WORKING_DIRECTORY "${CMAKE_SOURCE_DIR}/demos"
            COMMENT "Generating ${fn} to ${ASSETS_PATH}/${fn}.dat"
            )
    list(APPEND out_data_files ${ASSETS_PATH}/${fn}.dat)
endforeach()

add_custom_command(OUTPUT ${ASSETS_PATH}/config.tic.dat
        COMMAND bin2txt config.tic ${ASSETS_PATH}/config.tic.dat -z
        DEPENDS ${CMAKE_SOURCE_DIR}/config.tic
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
        COMMENT "Generating config.tic.dat")
list(APPEND out_data_files ${ASSETS_PATH}/config.tic.dat)

add_custom_command(OUTPUT ${ASSETS_PATH}/index.html.dat
        COMMAND bin2txt index.html ${ASSETS_PATH}/index.html.dat -z
        DEPENDS ${CMAKE_SOURCE_DIR}/build/html/index.html
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/build/html
        COMMENT "Generating index.html.dat")
list(APPEND out_data_files ${ASSETS_PATH}/index.html.dat)

add_custom_target(GenerateDataFiles DEPENDS ${out_data_files})

add_dependencies(tic GenerateDataFiles)