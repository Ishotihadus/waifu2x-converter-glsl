cmake_minimum_required(VERSION 3.1.0 FATAL_ERROR)

include(CheckCXXCompilerFlag)

project(waifu2x-converter-glsl)

set(CMAKE_CXX_STANDARD 11)

find_package(PkgConfig REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)
find_package(OpenCV 3 REQUIRED)
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)
pkg_search_module(GLFW REQUIRED glfw3)

include_directories(
  include
  ${OpenGL_INCLUDE_DIRS}
  ${GLEW_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${GLFW_INCLUDE_DIRS})

add_executable(waifu2x-converter-glsl
  src/convertRoutine.cpp
  src/filterGL.cpp
  src/main.cpp
  src/modelHandler.cpp
  src/modelHandlerFilter.cpp
  src/modelHandlerFilterGL.cpp)

target_link_libraries(waifu2x-converter-glsl
  ${OPENGL_gl_LIBRARY}
  ${GLEW_LIBRARIES}
  ${OpenCV_LIBRARIES}
  ${GLFW_LIBRARIES}
  Threads::Threads)
