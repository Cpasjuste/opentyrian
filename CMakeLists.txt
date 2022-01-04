cmake_minimum_required(VERSION 3.0)

project(opentyrian)

include(FindPkgConfig)

# source files
file(GLOB SOURCES src/*.c)

# SDL2
pkg_search_module(SDL2 REQUIRED sdl2)
list(APPEND INCLUDES ${SDL2_INCLUDE_DIRS})
list(APPEND CFLAGS ${SDL2_CFLAGS})
list(APPEND LDFLAGS ${SDL2_LIBRARIES})

# common flags
list(APPEND CFLAGS
        -DTARGET_UNIX
        -DOPENTYRIAN_VERSION="1.0.0"
        )

if (PS4)
    if (CMAKE_BUILD_TYPE STREQUAL "Debug")
        list(APPEND CFLAGS
                -DTYRIAN_DIR="/data/opentyrian/data"
                -DUSER_DIR="/data/opentyrian"
                )
    else ()
        list(APPEND CFLAGS
                -DTYRIAN_DIR="/app0/assets"
                -DUSER_DIR="/data/opentyrian"
                )
    endif ()
else ()
    list(APPEND CFLAGS
            -DTYRIAN_DIR="data"
            -DUSER_DIR="."
            )
endif ()

add_executable(${PROJECT_NAME} ${SOURCES})
target_compile_options(${PROJECT_NAME} PUBLIC ${CFLAGS})
target_link_libraries(${PROJECT_NAME} ${LDFLAGS} m)

if (PS4)
    add_self(${PROJECT_NAME})
    add_pkg(${PROJECT_NAME} "${CMAKE_SOURCE_DIR}/pkg" "OPEN00001" "OpenTyrian @ cpasjuste" "01.01")
endif ()