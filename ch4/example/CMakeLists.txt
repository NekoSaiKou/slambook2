find_package(Pangolin REQUIRED)
include_directories(${Pangolin_INCLUDE_DIRS})
find_package(Sophus REQUIRED)

add_executable(trajectoryError trajectoryError.cpp)
target_link_libraries(trajectoryError ${Pangolin_LIBRARIES} Sophus::Sophus)
