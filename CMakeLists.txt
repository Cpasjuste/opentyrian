cmake_minimum_required(VERSION 3.0)

project(opentyrian)

include(FindPkgConfig)
# SDL2
pkg_search_module(SDL2 REQUIRED sdl2)
list(APPEND INCLUDES ${SDL2_INCLUDE_DIRS})
list(APPEND CFLAGS ${SDL2_CFLAGS})
list(APPEND LDFLAGS ${SDL2_LIBRARIES})

list(APPEND CFLAGS
        -DTARGET_UNIX
        -DTYRIAN_DIR="/app0/assets"
        -DUSER_DIR="/data/opentyrian"
        -DOPENTYRIAN_VERSION="1.0.0"
        )

file(GLOB SOURCES src/*.c)

add_executable(${PROJECT_NAME} ${SOURCES})
add_self(${PROJECT_NAME})
add_pkg(${PROJECT_NAME} "${CMAKE_SOURCE_DIR}/pkg" "OPEN00001" "OpenTyrian @ cpasjuste" "01.00")
target_compile_options(${PROJECT_NAME} PUBLIC ${CFLAGS})
target_link_libraries(${PROJECT_NAME} ${LDFLAGS})