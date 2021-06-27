cmake_minimum_required(VERSION 2.6 FATAL_ERROR)
project(recipe-04 LANGUAGES CXX)


add_executable(arch-dependent arch_dependent.cpp)


#CMake的CMAKE_SIZEOF_VOID_P变量会告诉我们CPU是32位还是64位。我们通过状态消息让用户知道检测到的大小，并设置预处理器定义:
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    target_compile_definitions(arch-dependent PUBLIC "IS_64_BIT_ARCH")
    message(STATUS "Target is 64 bits")
else()
    target_compile_definitions(arch-dependent PUBLIC "IS_32_BIT_ARCH")
    message(STATUS "Target is 32 bits")
endif()


#CMake定义了CMAKE_HOST_SYSTEM_PROCESSOR变量，以包含当前运行的处理器的名称。可以设置为“i386”、“i686”、“x86_64”、“AMD64”等等，
#当然，这取决于当前的CPU。
#CMAKE_SIZEOF_VOID_P为void指针的大小。我们可以在CMake配置时进行查询，以便修改目标或目标编译定义。
#可以基于检测到的主机处理器体系结构，使用预处理器定义，确定需要编译的分支源代码。正如在前面的示例中所讨论的，编写新代码时应该避免这种依赖，但在处理遗留代码或交叉编译时，这种依赖是有用的，交叉编译会在第13章进行讨论。

if(CMAKE_HOST_SYSTEM_PROCESSOR MATCHES "i386")
    message(STATUS "i386 architecture detected")
elseif(CMAKE_HOST_SYSTEM_PROCESSOR MATCHES "i686")
    message(STATUS "i686 architecture detected")
elseif(CMAKE_HOST_SYSTEM_PROCESSOR MATCHES "x86_64")
    message(STATUS "x86_64 architecture detected")
else()
    message(STATUS "host processor architecture is unknown")
endif()

target_compile_definitions(arch-dependent
  PUBLIC "ARCHITECTURE=${CMAKE_HOST_SYSTEM_PROCESSOR}"
  )