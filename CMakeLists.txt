cmake_minimum_required(VERSION 3.5)
project(robin-hood-hashing
    VERSION 3.10.0
    LANGUAGES CXX
)

# generate compile_commands.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(RH_cxx_standard "14" CACHE STRING "C++ standard, e.g. 11, 14, 17")

set(CMAKE_CXX_STANDARD ${RH_cxx_standard})
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# creates a library robin_hood which is an interface (header files only)

set(LIB_NAME "robin_hood")
set(LIB_NAMESPACE "robin_hood")

add_library(${LIB_NAME} INTERFACE)
add_library(${LIB_NAMESPACE}::${LIB_NAME} ALIAS ${LIB_NAME})

target_include_directories(robin_hood INTERFACE
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/src/include>
    $<INSTALL_INTERFACE:include>
)

include(GNUInstallDirs)

install(
    TARGETS ${LIB_NAME}
    EXPORT ${LIB_NAME}-targets
)

install(
    FILES src/include/robin_hood.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

configure_file(
    cmake/config.cmake.in
    ${LIB_NAME}Config.cmake
    @ONLY
)

install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/${LIB_NAME}Config.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${LIB_NAME}
)

install(
    EXPORT ${LIB_NAME}-targets
    NAMESPACE ${LIB_NAMESPACE}::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${LIB_NAME}
)
