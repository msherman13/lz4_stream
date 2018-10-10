cmake_minimum_required(VERSION 3.1)
project(lz4_stream)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")

if(MSVC)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W3 /WX")
else()
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Werror")
endif()

set(CMAKE_CXX_FLAGS_COVERAGE
  "${GCC_DEBUG_FLAGS} -fprofile-arcs -ftest-coverage"
  CACHE STRING "Flags used by the C++ compiler during coverage builds."
  FORCE)
set(CMAKE_EXE_LINKER_FLAGS_COVERAGE
  "${CMAKE_EXE_LINKER_FLAGS} --coverage"
  CACHE STRING "Flags used for linking binaries during coverage builds."
  FORCE)
mark_as_advanced(
  CMAKE_CXX_FLAGS_COVERAGE
  CMAKE_EXE_LINKER_FLAGS_COVERAGE)

find_package(LZ4 REQUIRED)

add_library(lz4_stream STATIC lz4_input_stream.cpp lz4_output_stream.cpp)
target_link_libraries(lz4_stream lz4)

add_executable(lz4_compress lz4_compress.cpp)
target_link_libraries(lz4_compress lz4_stream)

add_executable(lz4_decompress lz4_decompress.cpp)
target_link_libraries(lz4_decompress lz4_stream)
