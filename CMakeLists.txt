cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

set(PROJECT_NAME "NanoLog")
project(${PROJECT_NAME})

include(GNUInstallDirs)

option(BUILD_SHARED_LIBS "build shared libs" OFF)

add_subdirectory(src)