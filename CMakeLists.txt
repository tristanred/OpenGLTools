cmake_minimum_required(VERSION 3.9)
project(OpenGLTools)

set(CMAKE_CXX_STANDARD 11)

file(GLOB SOURCES "src/*.cpp")
file(GLOB HEADERS "src/*.h")


file(GLOB SRC_GLAD "lib/glad/src/glad.c")
include_directories("lib/glad/include")

IF(WIN32)
    add_definitions("-D_WINDOWS")
    add_definitions("-D_UNICODE")
    add_definitions("/Zc:strictStrings-")

    add_definitions(-D_CRT_SECURE_NO_WARNINGS)

ENDIF(WIN32)


add_executable(GLTools ${SOURCES} ${SRC_GLAD})


add_subdirectory("lib/libtech")
include_directories("lib/libtech/include")
target_link_libraries(GLTools LIBTECHD)

add_subdirectory("lib/glfw")
include_directories("lib/glfw/include")
target_link_libraries(GLTools glfw)

#set(CMAKE_EXE_LINKER_FLAGS "-lGL -lX11 -lpthread -lXrandr -lXi")
