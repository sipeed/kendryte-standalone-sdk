# DO NOT MODIFY THIS FILE, IT WILL BE OVERRIDE!!!

# set this will supress some warnings
set(BUILDING_SDK "yes" CACHE INTERNAL "")

# basic config
if (NOT PROJ)
    get_filename_component(PROJ ${CMAKE_CURRENT_BINARY_DIR} DIRECTORY)
    get_filename_component(PROJ ${PROJ} NAME)
    string(REPLACE " " "_" PROJ ${PROJ})
    message(STATUS "PROJ not set, use ${PROJ} as PROJ. Also, you can set it manually. e.g. -DPROJ=hello_world")
else()
    message("PROJ = ${PROJ}")
endif ()
cmake_minimum_required(VERSION 3.0)
include(./cmake/common.cmake)
project(${PROJ} C CXX ASM)

# config self use headers
include(./cmake/macros.internal.cmake)
header_directories(${SDK_ROOT}/lib)

if (NOT EXISTS ${SDK_ROOT}/src/${PROJ}/project.cmake)
    header_directories(src/${PROJ})
endif ()

header_directories(kendryte-standalone-demo/${PROJ})
# build library first
add_subdirectory(lib)

# compile project
if (NOT EXISTS ${SDK_ROOT}/src/${PROJ}/project.cmake)
    add_source_files(src/${PROJ}/*.c src/${PROJ}/*.s src/${PROJ}/*.S src/${PROJ}/*.cpp)
endif ()

add_source_files(kendryte-standalone-demo/${PROJ}/*.c kendryte-standalone-demo/${PROJ}/*.s kendryte-standalone-demo/${PROJ}/*.S kendryte-standalone-demo/${PROJ}/*.cpp)
include(./cmake/executable.cmake)

