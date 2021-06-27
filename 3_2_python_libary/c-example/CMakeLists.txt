# set minimum cmake version
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

# project name and language
project(recipe-02 LANGUAGES C)

#set(CMAKE_C_STANDARD 99)
#set(CMAKE_C_EXTENSIONS OFF)
#set(CMAKE_C_STANDARD_REQUIRED ON)

#PYTHON_LIBRARY：指向Python库的路径
#PYTHON_INCLUDE_DIR：Python.h所在的路径

find_package(PythonInterp REQUIRED)
find_package(PythonLibs ${PYTHON_VERSION_MAJOR}.${PYTHON_VERSION_MINOR} EXACT REQUIRED)

message("----PYTHON_INCLUDE_DIRS = ${PYTHON_INCLUDE_DIRS}")
message("----PYTHON_LIBRARIES = ${PYTHON_LIBRARIES}")

add_executable(hello-embedded-python hello-embedded-python.c)

#include_directories(hello-embedded-python
target_include_directories(hello-embedded-python
  PRIVATE
    ${PYTHON_INCLUDE_DIRS}
  )

target_link_libraries(hello-embedded-python
  PRIVATE
    ${PYTHON_LIBRARIES}
  )

#要先加到path里面
#export PATH=$PATH:/usr/local/python3/lib/:/usr/local/python3/include/python3.7m/