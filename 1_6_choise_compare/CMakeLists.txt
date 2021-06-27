cmake_minimum_required(VERSION 2.6)

project(recipe-06 LANGUAGES C CXX)

message(STATUS "Is the C++ compiler loaded? ${CMAKE_CXX_COMPILER_LOADED}")

if(CMAKE_CXX_COMPILER_LOADED)
    message(STATUS "1.The C++ compiler ID is: CMAKE_CXX_COMPILER_ID = ${CMAKE_CXX_COMPILER_ID}")
    message(STATUS "1.Is the C++ from GNU? CMAKE_COMPILER_IS_GNUCXX = ${CMAKE_COMPILER_IS_GNUCXX}")
    message(STATUS "1.The C++ compiler version is: CMAKE_CXX_COMPILER_VERSION = ${CMAKE_CXX_COMPILER_VERSION}")
endif()


message(STATUS "Is the C compiler loaded? ${CMAKE_C_COMPILER_LOADED}")
if(CMAKE_C_COMPILER_LOADED)
    message(STATUS "2.The C compiler ID is: CMAKE_C_COMPILER_ID = ${CMAKE_C_COMPILER_ID}")
    message(STATUS "2.Is the C from GNU? CMAKE_COMPILER_IS_GNUCC = ${CMAKE_COMPILER_IS_GNUCC}")
    message(STATUS "2.The C compiler version is: CMAKE_C_COMPILER_VERSION = ${CMAKE_C_COMPILER_VERSION}")
endif()