cmake_minimum_required(VERSION 3.9)
project(OpenGLTools)

set(CMAKE_CXX_STANDARD 11)

# Include the Libtech project
add_subdirectory("lib/libtech")
include_directories("lib/libtech/include")

# Add the project sources
file(GLOB SOURCES "src/*.cpp")
file(GLOB HEADERS "src/*.h")

# Include GLAD for OpenGL
IF(LIBTECH_SUPPORT_OPENGL)
    file(GLOB SRC_GLAD "lib/libtech/lib/glad/src/glad.c")
    include_directories("lib/libtech/lib/glad/include")
ELSE()
    file(GLOB SRC_GLAD "lib/glad/src/glad.c")
    include_directories("lib/glad/include")
ENDIF(LIBTECH_SUPPORT_OPENGL)


# TODO : Find out why these are included
IF(WIN32)
    add_definitions("-D_WINDOWS")
    add_definitions("-D_UNICODE")
    add_definitions("/Zc:strictStrings-")

    add_definitions(-D_CRT_SECURE_NO_WARNINGS)
ENDIF(WIN32)

# Declare the output executable
add_executable(GLTools ${SOURCES} ${SRC_GLAD})

# Link Libtech to the main project
target_link_libraries(GLTools LIBTECHD)

IF(LIBTECH_SUPPORT_OPENGL)
    include_directories("lib/libtech/lib/glfw/include")
    target_link_libraries(GLTools glfw)

    add_definitions(-DLIBTECH_BUILD_OPENGL)
ELSE()
    add_subdirectory("lib/glfw")
    include_directories("lib/glfw/include")
    target_link_libraries(GLTools glfw)
ENDIF(LIBTECH_SUPPORT_OPENGL)

# Copy the LIBTECH dll to the output dir
IF(WIN32)
    add_custom_command(
        TARGET GLTools POST_BUILD 
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${PROJECT_SOURCE_DIR}/build/lib/libtech/$(Configuration)/LIBTECHD.dll"
        $<TARGET_FILE_DIR:GLTools>)
ENDIF(WIN32)


#set(CMAKE_EXE_LINKER_FLAGS "-lGL -lX11 -lpthread -lXrandr -lXi")
