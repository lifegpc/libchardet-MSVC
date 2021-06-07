cmake_minimum_required(VERSION 3.0)

set(CHARDET_VERSION_MAJOR 1)
set(CHARDET_VERSION_MINOR 0)
set(CHARDET_VERSION_MICRO 7)
set(CHARDET_VERSION ${CHARDET_VERSION_MAJOR}.${CHARDET_VERSION_MINOR}.${CHARDET_VERSION_MICRO})

project(libchardet VERSION ${CHARDET_VERSION})

option(BUILD_SHARED_LIBS "Build shared library" ON)
option(BUILD_STATIC_LIBS "Build static library" ON)
option(BUILD_TESTING "Build test" ON)

include(CheckIncludeFiles)

if(WIN32)
    set(HAVE_DLL_EXPORT 1)
endif()

include_directories(${CMAKE_SOURCE_DIR})
include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CMAKE_SOURCE_DIR}/src)

configure_file(cmake-config.h.in ${CMAKE_SOURCE_DIR}/chardet-config.h)
if(BUILD_SHARED_LIBS AND MSVC)
    configure_file(dll.rc.in ${CMAKE_SOURCE_DIR}/dll.rc)
endif()

add_definitions(-DHAVE_CONFIG_H)

add_subdirectory(src)

if(BUILD_TESTING)
    enable_testing()
    add_subdirectory(test)
endif()
