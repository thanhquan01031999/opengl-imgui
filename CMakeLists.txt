cmake_minimum_required(VERSION 3.10)
project(hello_window VERSION 1.0.0)
cmake_policy(SET CMP0072 NEW)

find_package(OpenGL REQUIRED)
find_package(glfw3 REQUIRED)
set( THIRDPARTY_DIR ${CMAKE_CURRENT_LIST_DIR}/dependencies )
set( IMGUI_SRC_DIR ${THIRDPARTY_DIR}/imgui )

set( IMGUI_PUBLIC_HEADERS ${IMGUI_SRC_DIR}/imgui.h )

set( IMGUI_SOURCES
    ${IMGUI_SRC_DIR}/imgui.cpp
    ${IMGUI_SRC_DIR}/imgui_draw.cpp
    ${IMGUI_SRC_DIR}/imgui_demo.cpp
    ${IMGUI_SRC_DIR}/imgui_widgets.cpp
     )

add_library( imgui STATIC ${IMGUI_SOURCES} )
target_include_directories( imgui PRIVATE ${THIRDPARTY_DIR} ${IMGUI_SRC_DIR} )
add_library(STB_IMAGE "src/stb_image.cpp")
set(LIBS ${LIBS} STB_IMAGE)

add_subdirectory( ${THIRDPARTY_DIR}/fmt )
add_subdirectory( ${THIRDPARTY_DIR}/glm )
configure_file(configuration/root_directory.h.in configuration/root_directory.h)
include_directories(${CMAKE_BINARY_DIR}/configuration)

add_executable(hello_window
    src/main.cpp 
    src/glad.c
    src/imgui_impl/opengl3.cpp
    src/imgui_impl/glfw.cpp
    src/imgui_impl/fonts.cpp
)

target_include_directories(hello_window 
    PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/include
    dependencies 
)

target_link_libraries(hello_window 
    glfw
    OpenGL::GL 
    dl
    imgui
    fmt::fmt
    glm
    ${LIBS}
)