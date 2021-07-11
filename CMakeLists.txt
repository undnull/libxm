cmake_minimum_required(VERSION 3.10)
project(xm LANGUAGES C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

add_library(xm STATIC
    "${CMAKE_CURRENT_LIST_DIR}/src/context.c"
    "${CMAKE_CURRENT_LIST_DIR}/src/load.c"
    "${CMAKE_CURRENT_LIST_DIR}/src/play.c"
    "${CMAKE_CURRENT_LIST_DIR}/src/xm.c")
target_compile_definitions(xm PRIVATE
    XM_DEFENSIVE=1              # Defensively check XM data for errors/inconsistencies
    XM_LINEAR_INTERPOLATION=0   # Use linear interpolation (CPU hungry)
    XM_RAMPING=1                # Enable ramping (smooth volume/panning transitions, CPU hungry)
    XM_LIBXMIZE_DELTA_SAMPLES=0
    XM_BIG_ENDIAN=0
    XM_STRINGS=0)
target_include_directories(xm PUBLIC "${CMAKE_CURRENT_LIST_DIR}/include")

# XM_DEBUG must be defined all the time.
# Again, why not to use ifdef? Annoying!
if(CMAKE_BUILD_TYPE MATCHES "Debug")
    message("-- XM_DEBUG is 1")
    target_compile_definitions(xm PRIVATE XM_DEBUG=1)
else()
    target_compile_definitions(xm PRIVATE XM_DEBUG=0)
endif()
