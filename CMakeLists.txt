cmake_minimum_required(VERSION 2.8)

include(CheckCXXCompilerFlag)

project(waifu2x-converter-glsl CXX)

CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
endif()

find_package(PkgConfig REQUIRED)
find_package(OpenGL REQUIRED)
find_package(GLEW REQUIRED)
find_package(OpenCV 3 REQUIRED)
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
  ${OPENGL_LIBRARIES}
  ${GLEW_LIBRARIES}
  ${OpenCV_LIBRARIES}
  ${GLFW_LIBRARIES})
