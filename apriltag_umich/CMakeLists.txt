cmake_minimum_required(VERSION 2.8)
project(apriltag_umich)

message(STATUS "Project: " ${PROJECT_NAME})
message(STATUS "Source: " ${PROJECT_SOURCE_DIR})
message(STATUS "Binary: " ${PROJECT_BINARY_DIR})

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
#link libraries
link_directories(${PROJECT_SOURCE_DIR}/lib)

include_directories(apriltag .)

find_package (Threads)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
message(STATUS "OpenCV: " ${OpenCV_INCLUDE_DIRS})

set(CMAKE_BUILD_TYPE Release)
set(CMAKE_C_FLAGS "-std=gnu99 -fPIC")
file(GLOB C_FILES ${PROJECT_SOURCE_DIR}/src/*.c)
list(REMOVE_ITEM C_FILES ${PROJECT_SOURCE_DIR}/src/tagtest.c)
add_library(${PROJECT_NAME} SHARED ${C_FILES})
target_link_libraries (${PROJECT_NAME} ${CMAKE_THREAD_LIBS_INIT})

#apriltag umich
add_executable(test_apriltag_umich src/test_apriltag_umich.cpp)
target_link_libraries(test_apriltag_umich ${OpenCV_LIBS})
target_link_libraries(test_apriltag_umich ${PROJECT_NAME})