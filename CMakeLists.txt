cmake_minimum_required(VERSION 3.10)
project(LINPACK LANGUAGES C)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin/)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)  
enable_testing()
add_executable(linpack src/linpack.c)
target_link_libraries(linpack m)

target_compile_options(linpack PRIVATE -O3 -march=native)

if(APPLE AND CMAKE_C_COMPILER_ID MATCHES "Clang")
    find_path(OMP_INCLUDE_DIR omp.h
        HINTS /opt/homebrew/opt/libomp/include /usr/local/opt/libomp/include)
    find_library(OMP_LIBRARY omp
        HINTS /opt/homebrew/opt/libomp/lib /usr/local/opt/libomp/lib)
    if(NOT OMP_INCLUDE_DIR OR NOT OMP_LIBRARY)
        message(FATAL_ERROR "Install OpenMP first: brew install libomp")
    endif()
    target_include_directories(linpack PRIVATE ${OMP_INCLUDE_DIR})
    target_compile_options(linpack PRIVATE -Xpreprocessor -fopenmp)
    target_link_libraries(linpack ${OMP_LIBRARY})
else()
    find_package(OpenMP REQUIRED COMPONENTS C)
    target_link_libraries(linpack OpenMP::OpenMP_C)
endif()

add_test(NAME init_test COMMAND linpack --array-size=200)

