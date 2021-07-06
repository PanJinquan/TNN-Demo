cmake_minimum_required(VERSION 3.5)
project(Detector)

add_compile_options(-fPIC) # fix Bug: can not be used when making a shared object
set(CMAKE_CXX_FLAGS "-Wall -std=c++11")
#set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG")
#set(CMAKE_CXX_FLAGS_DEBUG "-g")

# TNN set
# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds it for you.
# Gradle automatically packages shared libraries with your APK.
# build for platform

set(TNN_OPENCL_ENABLE ON CACHE BOOL "" FORCE)
set(TNN_CPU_ENABLE ON CACHE BOOL "" FORCE)
set(TNN_X86_ENABLE OFF CACHE BOOL "" FORCE)
set(TNN_QUANTIZATION_ENABLE OFF CACHE BOOL "" FORCE)
set(TNN_OPENMP_ENABLE ON CACHE BOOL "" FORCE)  # Multi-Thread

set(TNN_ROOT 3rdparty/TNN)
include_directories(${TNN_ROOT}/include)
include_directories(${TNN_ROOT}/third_party/opencl/include)
add_subdirectory(${TNN_ROOT}) # 添加外部项目文件夹
set(TNN -Wl,--whole-archive TNN -Wl,--no-whole-archive)# set TNN library
MESSAGE(STATUS "TNN_ROOT = ${TNN_ROOT}")

# Detector
include_directories(src)
set(SRC_LIST src/test.cpp)
add_library(dmcv SHARED ${SRC_LIST})
target_link_libraries(dmcv ${TNN})
MESSAGE(STATUS "DIR_SRCS = ${SRC_LIST}")

add_executable(Detector src/main.cpp)
target_link_libraries(Detector dmcv)


