cmake_minimum_required(VERSION 3.10)

project(ndisapi LANGUAGES CXX RC)  # Include RC for resource files

set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

set(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "Configs" FORCE)
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
endif()

set(PLATFORM "x64")

add_library(ndisapi STATIC)

target_include_directories(ndisapi PUBLIC include)

file(GLOB SOURCES
    "ndisapi/*.cpp"
    "ndisapi/*.h"
    "ndisapi/*.rc"
)

target_sources(ndisapi PRIVATE ${SOURCES})

target_compile_definitions(ndisapi PRIVATE 
    _LIB
    $<$<CONFIG:Debug>:_DEBUG>
    $<$<CONFIG:Release>:NDEBUG>
)

set_target_properties(ndisapi PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib/${PLATFORM}"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${PLATFORM}"
)