# Annotated Boilerplate CMakeList.txt 

#=================================

cmake_minimum_required(VERSION 3.16)
project(libxml_playground C)

set(CMAKE_C_STANDARD 11)


# complier options

# for unix

target_compile_options(myApp PRIVATE -Wall -Wextra -Wpedantic)

# for each system
if (CMAKE_SYSTEM_NAME STREQUAL "Windows")
    target_compile_options(myApp PRIVATE /W4)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
    target_compile_options(myApp PRIVATE -Wall -Wextra -Wpedantic)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    # other macOS-specific flags for Clang
endif()


# Method1: find a package
# => list packages built-in cmake
# cmake --help-module-list

find_package(LibXml2 REQUIRED)
add_executable(libxml_playground main.c)
target_compile_definitions(libxml_playground PUBLIC ${LIBXML2_DEFINITIONS})
target_include_directories(libxml_playground  PUBLIC ${LIBXML2_INCLUDE_DIRS})


# Method2: CMAKE with PkgConfig
# => To get all pkg-config availiable.
# pkg-config --list-all

find_package (PkgConfig REQUIRED)
pkg_check_modules (CAIRO REQUIRED cairo>=1.5)

include_directories(${CAIRO_INCLUDE_DIRS})
link_directories (${CAIRO_LIBRARY_DIRS})

add_executable(cairo_main main.cpp)

target_link_libraries(cairo_main PUBLIC
        ${CAIRO_LIBRARIES})

# Method3: modern find_library
find_library(LIBRARY_SDL sdl)

add_executable(sdl_main main.cpp)

if (LIBRARY_SDL)
    target_link_libraries(myApp PRIVATE ${LIBRARY_SDL})
else()
    # throw an error or enable compilation without the library
endif()

# ===

add_executable (gtk3-test main.c)

add_definitions (${GTK3_CFLAGS_OTHER}) <<<<<<<<<<<<<<<< CHECK IT!

target_link_libraries (gtk3-test ${GTK3_LIBRARIES})

target_link_libraries(libxml_playground ${LIBXML2_LIBRARIES}) # Linker

########################################
difference between find_package & pkg_check_modules

+ find_package is CMake’s very own mechanism for solving the same problem. For 
this to work you either need a CMake find script for the requested library 
(CMake already ships with a couple of those, but you can easily write your own) 
or alternatively a package config script provided by the requested library 
itself. In either case you might have to adjust your CMAKE_MODULE_PATH for 
CMake to be able to find the respective script.

+ pkg_search_module uses the pkg-config tool to determine the location of the 
requested library. This is mostly useful on systems where pkg-config is already 
in use, so you do not need to replicate all the information for CMake. Note 
that this approach has potential portability issues, since it requires 
pkg-config to be setup correctly on the build machine.

+ https://cmake.org/cmake/help/latest/command/find_library.html

# PKG config use folling path in OpenSUSE 15.2
pkg-config --variable pc_path pkg-config
/usr/local/lib64/pkgconfig:/usr/local/lib/pkgconfig:/usr/local/share/pkgconfig:/usr/lib64/pkgconfig:/usr/share/pkgconfig:/opt/kde3/lib64/pkgconfig

# copy one file
configure_file(<input> <output> COPYONLY)
