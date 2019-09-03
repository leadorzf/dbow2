cmake_minimum_required(VERSION 2.8)
project(dbow2)
include(ExternalProject)
set(OpenCV_DIR /opt/ros/kinetic/share/OpenCV-3.3.1-dev)
option(BUILD_DBoW2   "Build dbow2"            ON)
option(BUILD_Demo    "Build demo application" ON)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build." FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release"
    "MinSizeRel" "RelWithDebInfo")
endif()

set(HDRS
    include/BowVector.h
    include/FFREAK.h
    include/FORB.h 
    include/FClass.h
    include/FeatureVector.h
    include/ScoringObject.h
    include/TemplatedVocabulary.h
    include/Timestamp.h
    include/Random.h
    )

set(SRCS
  DBoW2/BowVector.cpp
  DBoW2/FORB.cpp   
  DBoW2/FFREAK.cpp  
  DBoW2/FeatureVector.cpp
  DBoW2/ScoringObject.cpp)

set(SRCS
  DUtils/Random.cpp
  DUtils/Timestamp.cpp)

set(DEPENDENCY_DIR ${CMAKE_CURRENT_BINARY_DIR}/dependencies)
set(DEPENDENCY_INSTALL_DIR ${DEPENDENCY_DIR}/install)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

if(BUILD_DBoW2)
  set(LIB_SHARED "SHARED")
  if(WIN32)
    set(LIB_SHARED "STATIC")
  endif(WIN32)
  add_library(${PROJECT_NAME} ${LIB_SHARED} ${SRCS})
  include_directories(include/)
  target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS})
endif(BUILD_DBoW2)

install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
if(BUILD_DBoW2)
  install(DIRECTORY include DESTINATION ${CMAKE_INSTALL_PREFIX}/include)
endif()
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/DBoW2Config.cmake"
  DESTINATION ${CMAKE_INSTALL_PREFIX}/include/${PROJECT_NAME})
install(FILES "${PROJECT_BINARY_DIR}/DBoW2Config.cmake" 
  DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/dbow2/)
install(DIRECTORY ${DEPENDENCY_INSTALL_DIR}/ DESTINATION ${CMAKE_INSTALL_PREFIX} OPTIONAL)
