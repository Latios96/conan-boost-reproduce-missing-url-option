cmake_minimum_required(VERSION 2.8)

set(CMAKE_CXX_STANDARD_REQUIRED ON)

if(NOT DEFINED CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE RelWithDebInfo)
endif()

project(crayg)
set(CMAKE_CXX_STANDARD 17)

enable_testing()

unset(ENV{CI})

set(CONAN_SYSTEM_INCLUDES ON)

if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
    message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
    file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/master/conan.cmake"
         "${CMAKE_BINARY_DIR}/conan.cmake")
endif()

include(${CMAKE_BINARY_DIR}/conan.cmake)

set(CMAKE_BUILD_TYPE_FOR_CONAN Release)
if(${CMAKE_BUILD_TYPE} STREQUAL Debug)
    set(CMAKE_BUILD_TYPE_FOR_CONAN Debug)
endif()
message(STATUS "Using BUILD_TYPE=${CMAKE_BUILD_TYPE_FOR_CONAN} for Conan dependencies")

conan_cmake_run(
    CONANFILE
    conanfile.txt
    BASIC_SETUP
    BUILD
    missing
    KEEP_RPATHS
    BUILD_TYPE
    ${CMAKE_BUILD_TYPE_FOR_CONAN})

