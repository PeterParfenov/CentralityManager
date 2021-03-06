# CMakeLists.txt for event package. It creates a library with dictionary and a main program
cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(CentralityManager)

# You need to tell CMake where to find the ROOT installation. This can be done in a number of ways:
#   - ROOT built with classic configure/make use the provided $ROOTSYS/etc/cmake/FindROOT.cmake
#   - ROOT built with CMake. Add in CMAKE_PREFIX_PATH the installation prefix for ROOT
list(APPEND CMAKE_PREFIX_PATH $ENV{ROOTSYS})

#---Locate the ROOT package and defines a number of variables (e.g. ROOT_INCLUDE_DIRS)
find_package(ROOT REQUIRED COMPONENTS MathCore RIO Hist Tree Net)

#---Define useful ROOT functions and macros (e.g. ROOT_GENERATE_DICTIONARY)
include(${ROOT_USE_FILE})

set(INCLUDE_DIRECTORIES
  ${CMAKE_SOURCE_DIR}
  ${ROOT_INCLUDE_DIRS}
)

include_directories(${INCLUDE_DIRECTORIES}
)
add_definitions(${ROOT_CXX_FLAGS})

set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_C_FLAGS "-O2")

set(CentralityManager_headers_files
  CentralityManager.h
)

set(CentralityManager_cxx_files
  CentralityManager.cxx
)

#---Generate dictionary for CentralityManager
ROOT_GENERATE_DICTIONARY(G__CentralityManager
  ${CentralityManager_headers_files}
LINKDEF CentralityManager.LinkDef.h)

#---Create a shared library with geneated dictionary for CentralityManager class
add_library(CentralityManager SHARED
  ${CentralityManager_cxx_files}
G__CentralityManager.cxx)

target_link_libraries(CentralityManager ${ROOT_LIBRARIES})

#---Create an executable file
add_executable(CMMain CentralityManagerMain.cpp)
target_link_libraries(CMMain CentralityManager)