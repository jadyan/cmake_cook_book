# set minimum cmake version
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

# project name and language
project(recipe-02 LANGUAGES CXX)




#CMake为目标操作系统定义了CMAKE_SYSTEM_NAME，因此不需要使用定制命令、工具或脚本来查询此信息。
#然后，可以使用此变量的值实现特定于操作系统的条件和解决方案。在具有uname命令的系统上，
#将此变量设置为uname -s的输出。该变量在macOS上设置为“Darwin”。在Linux和Windows上，它分别计算为“Linux”和“Windows”。
#我们了解了如何在特定的操作系统上执行特定的CMake代码。当然，应该尽量减少这种定制化行为，以便简化迁移到新平台的过程。

#NOTE:为了最小化从一个平台转移到另一个平台时的成本，应该避免直接使用Shell命令，还应该避免显式的路径分隔符(Linux和macOS上的前斜杠和Windows上的后斜杠)。
#CMake代码中只使用前斜杠作为路径分隔符，CMake将自动将它们转换为所涉及的操作系统环境。



message("-------${CMAKE_SYSTEM_NAME}")
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    message(STATUS "Configuring on/for Linux")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    message(STATUS "Configuring on/for macOS")
elseif(CMAKE_SYSTEM_NAME STREQUAL "Windows")
    message(STATUS "Configuring on/for Windows")
elseif(CMAKE_SYSTEM_NAME STREQUAL "AIX")
    message(STATUS "Configuring on/for IBM AIX")
else()
    message(STATUS "Configuring on/for ${CMAKE_SYSTEM_NAME}")
endif()


add_executable(hello-world hello-world.cpp)
