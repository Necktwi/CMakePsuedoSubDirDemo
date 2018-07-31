# Gowtham Kudupudi 23/06/2018
# 
# Further authors should be added above following a new line.
# Tab size is 3. Right margin is 80 characters away.
# 
# ©FerryFair
                                                                                
cmake_minimum_required (VERSION 3.5)
include (GNUInstallDirs)
project (CMakePsuedoSubDirDemo)
message ("Generating build procedure for ${PROJECT_NAME} ...")

message ("Extropolating build type ...")
string (
   REPLACE ${CMAKE_CURRENT_SOURCE_DIR} "" SRC_RLTV_BLD_DIR
   ${PROJECT_BINARY_DIR}
)
message ("SRC_RLTV_BLD_DIR: ${SRC_RLTV_BLD_DIR}")
string (REPLACE "/Build" "" BUILD_TARGET ${SRC_RLTV_BLD_DIR})
if (${SRC_RLTV_BLD_DIR})
   string (REPLACE "/build" "" BUILD_TARGET ${BUILD_TARGET})
endif ()
message ("BUILD_TARGET: ${BUILD_TARGET}")

if (NOT TARGET Demo1)
   add_subdirectory (
      ${CMAKE_CURRENT_SOURCE_DIR}/../Demo1
      ${CMAKE_CURRENT_SOURCE_DIR}/../Demo1/Build${BUILD_TARGET}
   )
endif ()

set (GCC_COVERAGE_COMPILE_FLAGS "${GCC_COVERAGE_COMPILE_FLAGS} -g -O0")

file(GLOB HEADERS [a-zA-Z0-9_]*.h)
file(GLOB SOURCES [a-zA-Z0-9_]*.cpp)
include_directories (${CMAKE_CURRENT_SOURCE_DIR}/../Demo1)
if(CMAKE_CONFIGURATION_TYPES)
   message ("Linking corresponding to the Configuration Name")
   link_directories (
      "${CMAKE_CURRENT_SOURCE_DIR}/../Demo1/Build${BUILD_TARGET}/$(ConfigurationName)"
   )
else ()
   link_directories (
      "${CMAKE_CURRENT_SOURCE_DIR}/../Demo1/Build${BUILD_TARGET}/"
   )
endif ()

add_executable(${PROJECT_NAME} ${SOURCES} ${HEADERS})
target_link_libraries(${PROJECT_NAME} Demo1)
