# Some functions require a certain version of cmake (for example file GLOB )
cmake_minimum_required(VERSION 2.8)

# Create the project
set(PROJECT_NAME fluidsim)
project(${PROJECT_NAME})

# Add the external module path
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake/modules)

# Find external packages

# --- 

# Add include directories

# --- include_directories(${SOMETHING}_INCLUDE_DIRS)

# Our own include directory
set(COMMON_INCLUDES ${PROJECT_SOURCE_DIR}/include)
include_directories(${COMMON_INCLUDES})

# Debugger flag
set(CMAKE_CXX_FLAGS "-g")

# Creating our own ${PROJECCT_NAME} library
# (Detach it from main function and include it to make it modular,
# for example for the use of testing).
file(GLOB ${PROJECCT_NAME}_lib_SOURCES ${PROJECT_SOURCE_DIR}/src/*.cpp)
file(GLOB ${PROJECCT_NAME}_lib_HEADERS ${PROJECT_SOURCE_DIR}/include/*.h)
# We do not want main.cpp in the library
file(GLOB to_remove ${PROJECT_SOURCE_DIR}/src/main.cpp)
list(REMOVE_ITEM ${PROJECCT_NAME}_lib_SOURCES ${to_remove})
list(REMOVE_ITEM ${PROJECCT_NAME}_lib_HEADERS ${to_remove})
add_library(${PROJECT_NAME}_lib
	${${PROJECCT_NAME}_lib_SOURCES}
	${${PROJECCT_NAME}_lib_HEADERS})

# Create the executable
add_executable(${PROJECT_NAME} src/main.cpp)

# C++11 compatability
set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "-std=c++11")

# Link all the libraries (including our own)
target_link_libraries(${PROJECT_NAME}
	${PROJECT_NAME}_lib
	)