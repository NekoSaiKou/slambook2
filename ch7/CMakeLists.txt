cmake_minimum_required(VERSION 2.8)
project(vo1)

set(CMAKE_BUILD_TYPE "Release")
add_definitions("-DENABLE_SSE")
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 ${SSE_FLAGS} -msse4")

cmake_policy(SET CMP0074 NEW)

find_package(OpenCV 4 REQUIRED)
find_package(Sophus REQUIRED)

# Find and set link of g2o
list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
set(G2O_ROOT "../3rdparty/g2o")
find_package(G2O REQUIRED)
if(NOT G2O_FOUND)
        message(FATAL_ERROR "G2O not found")
else()
        message(STATUS "Found g2o: ${G2O_INCLUDE_DIRS}")
endif()
link_directories(${G2O_INCLUDE_DIRS}/lib)

include_directories(
        ${OpenCV_INCLUDE_DIRS}
        ${G2O_INCLUDE_DIRS}
        ${Sophus_INCLUDE_DIRS}
        ${EIGEN3_INCLUDE_DIR}
)

add_executable(orb_cv orb_cv.cpp)
target_link_libraries(orb_cv ${OpenCV_LIBS})

add_executable(orb_self orb_self.cpp)
target_link_libraries(orb_self ${OpenCV_LIBS})

# add_executable( pose_estimation_2d2d pose_estimation_2d2d.cpp extra.cpp ) # use this if in OpenCV2 
add_executable(pose_estimation_2d2d pose_estimation_2d2d.cpp)
target_link_libraries(pose_estimation_2d2d ${OpenCV_LIBS})

# # add_executable( triangulation triangulation.cpp extra.cpp) # use this if in opencv2
add_executable(triangulation triangulation.cpp)
target_link_libraries(triangulation ${OpenCV_LIBS})

add_executable(pose_estimation_3d2d pose_estimation_3d2d.cpp)
target_link_libraries(pose_estimation_3d2d
        g2o_core g2o_stuff
        Sophus::Sophus
        ${OpenCV_LIBS})

add_executable(pose_estimation_3d3d pose_estimation_3d3d.cpp)
target_link_libraries(pose_estimation_3d3d
        g2o_core g2o_stuff
        Sophus::Sophus
        ${OpenCV_LIBS})
