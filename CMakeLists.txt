cmake_minimum_required(VERSION 2.8)
project(Polycube)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(OpenVolumeMesh)
include_directories(OpenVolumeMesh/src)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/OpenVolumeMesh/src)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/libHexEx/cmake)
add_definitions(-DHEXEX_VERBOSE)
add_subdirectory(libHexEx)
include_directories(libHexEx/src)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/libHexEx/src)

add_subdirectory(ultimaille)
include_directories(ultimaille ultimaille/ext)

find_package(OpenMP)
if(OPENMP_FOUND)
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()

if (WIN32) 
	set(BUILD_SHARED_LIBS OFF)
endif()


add_executable(polycube main.cpp HexExWrapper.h HexExWrapper.cpp)
target_link_libraries(polycube ultimaille OpenNL_psm OpenVolumeMesh HexEx)


if(OpenMP_CXX_FOUND)
    target_link_libraries(polycube OpenMP::OpenMP_CXX)
endif()

