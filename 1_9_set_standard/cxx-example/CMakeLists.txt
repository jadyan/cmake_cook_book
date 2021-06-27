# set minimum cmake version
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

# project name and language
project(recipe-09 LANGUAGES CXX)

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

add_library(animals
  SHARED
    Animal.cpp
    Animal.hpp
    Cat.cpp
    Cat.hpp
    Dog.cpp
    Dog.hpp
    Factory.hpp
  )


#CXX_STANDARD会设置我们想要的标准。
#CXX_EXTENSIONS告诉CMake，只启用ISO C++标准的编译器标志，而不使用特定编译器的扩展。
#CXX_STANDARD_REQUIRED指定所选标准的版本。如果这个版本不可用，CMake将停止配置并出现错误。当这个属性被设置为OFF时，
#CMake将寻找下一个标准的最新版本，直到一个合适的标志。这意味着，首先查找C++14，然后是C++11，然后是C++98。（译者注：目前会从C++20或C++17开始查找）

#POSITION_INDEPENDENT_CODE
#是否创建与位置无关的目标。
#POSITION_INDEPENDENT_CODE属性确定是否创建位置无关的可行性文件或共享库。对于SHARED和MODULE库目标，此属性默认为True，否则为False。如果在创建目标时设置了此属性，则该属性由CMAKE_POSITION_INDEPENDENT_CODE变量的值初始化。
set_target_properties(animals
  PROPERTIES
    CXX_STANDARD 14
    CXX_EXTENSIONS OFF
    CXX_STANDARD_REQUIRED Off
    POSITION_INDEPENDENT_CODE 1
  )

add_executable(animal-farm animal-farm.cpp)

set_target_properties(animal-farm
  PROPERTIES
    CXX_STANDARD 14
    CXX_EXTENSIONS OFF
    CXX_STANDARD_REQUIRED ON
  )

target_link_libraries(animal-farm animals)
