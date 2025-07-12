cmake_minimum_required(VERSION 3.16)
project(ImGUIPlayground LANGUAGES CXX)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find OpenGL from the system
find_package(OpenGL REQUIRED)

add_subdirectory(external/glfw)

# Link with DearImGUI by making a lib
file(GLOB DEAR_IM_GUI_LIB_FILES ./external/DearImGUI/*.cpp)

set(DEAR_IM_GUI_BACKENDS
    ${CMAKE_CURRENT_SOURCE_DIR}/external/DearImGUI/backends/imgui_impl_glfw.cpp
    ${CMAKE_CURRENT_SOURCE_DIR}/external/DearImGUI/backends/imgui_impl_opengl3.cpp
)

add_library(ImGUILib STATIC ${DEAR_IM_GUI_LIB_FILES} ${DEAR_IM_GUI_BACKENDS})

target_include_directories(
  ImGUILib
  PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/external/DearImGUI
         ${CMAKE_CURRENT_SOURCE_DIR}/external/DearImGUI/backends
         ${CMAKE_CURRENT_SOURCE_DIR}/external/glfw/include)

target_link_libraries(ImGUILib PUBLIC glfw OpenGL::GL)

file(GLOB APPLICATION_ON_IMGUI ${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp)

add_executable(ImGUIPlayground ${APPLICATION_ON_IMGUI})
target_include_directories(ImGUIPlayground
                           PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)

target_link_libraries(ImGUIPlayground PRIVATE ImGUILib)
