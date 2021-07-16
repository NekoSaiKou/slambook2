cmake_minimum_required(VERSION 2.8)

project(bundle_adjustment)
set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_FLAGS "-O3")

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
set(G2O_ROOT "../3rdparty/g2o")
find_package(G2O REQUIRED)
if(NOT G2O_FOUND)
        message(FATAL_ERROR "G2O not found")
else()
        message(STATUS "Found g2o: ${G2O_INCLUDE_DIRS}")
endif()
link_directories(${G2O_INCLUDE_DIRS}/lib)

Find_Package(Eigen3 REQUIRED)
#Find_Package(Ceres REQUIRED)
Find_Package(Sophus REQUIRED)
#Find_Package(CSparse REQUIRED)

message("" ${EIGEN3_INCLUDE_DIR})

#SET(G2O_LIBS g2o_csparse_extension g2o_stuff g2o_core cxsparse)
SET(G2O_LIBS g2o_stuff g2o_core)
include_directories(
        ${G2O_INCLUDE_DIRS}
        ${Sophus_INCLUDE_DIRS}
        ${EIGEN3_INCLUDE_DIR}
        ${CSPARSE_INCLUDE_DIR}
)

add_library(bal_common common.cpp)
add_executable(bundle_adjustment_g2o bundle_adjustment_g2o.cpp)
target_link_libraries(bundle_adjustment_g2o ${G2O_LIBS} Sophus::Sophus bal_common)

#add_executable(bundle_adjustment_ceres bundle_adjustment_ceres.cpp)
#target_link_libraries(bundle_adjustment_ceres ${CERES_LIBRARIES} bal_common)
