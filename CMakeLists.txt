cmake_minimum_required(VERSION 3.5)
project(LINPACK LANGUAGES C)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin/)

add_executable(linpack src/linpack.c)
