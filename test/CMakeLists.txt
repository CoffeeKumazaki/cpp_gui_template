## Use the variable PROJECT_NAME for changing the target name
get_filename_component(DIR_ONE_ABOVE ../ ABSOLUTE)
get_filename_component(ProjectId ${DIR_ONE_ABOVE} NAME)
string(REPLACE " " "_" ProjectId ${ProjectId})
set( PROJECT_NAME ${ProjectId} )

cmake_minimum_required(VERSION 3.1)
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
project(${PROJECT_NAME})

# include_directories(include)

find_package(OpenGL REQUIRED)
add_definitions(-DIMGUI_IMPL_OPENGL_LOADER_GLEW)
include_directories(/usr/local/opt/glew/include)
include_directories(/usr/local/opt/glfw/include)
link_directories(/usr/local/lib)

# for m1 mac
include_directories(/opt/homebrew/opt/glew/include)
include_directories(/opt/homebrew/opt/glfw/include)
link_directories(/opt/homebrew/lib)

include_directories(../include)

## add imgui as a library
add_library(imgui STATIC
  src/imgui/imgui.cpp
  src/imgui/imgui_demo.cpp
  src/imgui/imgui_draw.cpp
  src/imgui/imgui_widgets.cpp
  src/imgui/imgui_impl_glfw.cpp
  src/imgui/imgui_impl_opengl3.cpp
)

## Define the executable
set(SRCS 
  src/main.cpp
)
add_executable(${PROJECT_NAME} ${SRCS})
target_include_directories(${PROJECT_NAME} PUBLIC
  include
)
target_link_libraries(${PROJECT_NAME} 
  imgui
  glew
  glfw 
  ${OPENGL_gl_LIBRARY} 
  ${PROJECT_NAME}_lib
)
target_compile_definitions(${PROJECT_NAME} PRIVATE PROJECT_NAME="${PROJECT_NAME}")
