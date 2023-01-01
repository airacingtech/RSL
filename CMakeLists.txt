cmake_minimum_required(VERSION 3.16)
project(rsl VERSION 0.2.1 LANGUAGES CXX DESCRIPTION "ROS Support Library")

set(CMAKE_CXX_EXTENSIONS OFF)

find_package(ament_cmake_auto REQUIRED)
find_package(eigen3_cmake_module REQUIRED)
find_package(Eigen3 REQUIRED)
include_directories(SYSTEM ${EIGEN3_INCLUDE_DIRS})

ament_auto_find_build_dependencies(REQUIRED
  ${${PROJECT_NAME}_BUILD_DEPENDS}
  ${${PROJECT_NAME}_BUILDTOOL_DEPENDS}
)

# Default to shared libraries to work around the Debian ecosystem's inability to specify -DBUILD_SHARED_LIBS=ON at configuration :(
option(BUILD_SHARED_LIBS "Build shared libraries" ON)

ament_auto_add_library(${PROJECT_NAME} SHARED 
  src/parameter_validators.cpp
  src/random.cpp
)

target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include/rsl>
)

install(
    DIRECTORY include/
    DESTINATION include/rsl
)

ament_auto_package()
