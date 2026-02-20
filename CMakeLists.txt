# Aseprite TGA Library
# Copyright (C) 2020-2021  Igara Studio S.A.

cmake_minimum_required(VERSION 3.20)

project(tga VERSION 1.0.0 LANGUAGES CXX)

# We need C++11 because we use the "override" keyword
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

add_library(tga STATIC
    decoder.cpp
    encoder.cpp
    image_iterator.cpp
    stdio.cpp)

target_include_directories(tga
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

add_library(tga::tga ALIAS tga)

set_target_properties(tga PROPERTIES PUBLIC_HEADER "tga.h")

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

install(EXPORT tgaTargets
        NAMESPACE tga::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/tga)

configure_package_config_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/cmake/tgaConfig.cmake.in
        ${CMAKE_CURRENT_BINARY_DIR}/tgaConfig.cmake
        INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/tga
)

write_basic_package_version_file(
        ${CMAKE_CURRENT_BINARY_DIR}/tgaConfigVersion.cmake
        VERSION ${PROJECT_VERSION}
        COMPATIBILITY SameMajorVersion
)

install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/tgaConfigVersion.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/tga
)

install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/tgaConfig.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/tga
)

install(TARGETS tga
        EXPORT tgaTargets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
