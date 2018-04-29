cmake_minimum_required(VERSION 3.9)
project(OpenGLTools)

set(CMAKE_CXX_STANDARD 11)

file(GLOB SOURCES "src/*.cpp")
file(GLOB HEADERS "src/*.h")

IF(WIN32)
    add_definitions("-D_WINDOWS")
    add_definitions("-D_UNICODE")
    add_definitions("/Zc:strictStrings-")

    add_definitions(-D_CRT_SECURE_NO_WARNINGS)

ENDIF(WIN32)


add_executable(GLTools ${SOURCES})


add_subdirectory("libtech")
include_directories("libtech/include")
target_link_libraries(GLTools LIBTECHD)