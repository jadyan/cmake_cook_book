#2.5 检测处理器指令集
# set minimum cmake version
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

# project name and language
project(recipe-05 LANGUAGES CXX)

# define executable
add_executable(processor-info "")

# and its source file
target_sources(processor-info
  PRIVATE
    processor-info.cpp
  )

# and its include directories
target_include_directories(processor-info
  PRIVATE
    ${PROJECT_BINARY_DIR}
  )

foreach(key
  IN ITEMS
    NUMBER_OF_LOGICAL_CORES
    NUMBER_OF_PHYSICAL_CORES
    TOTAL_VIRTUAL_MEMORY
    AVAILABLE_VIRTUAL_MEMORY
    TOTAL_PHYSICAL_MEMORY
    AVAILABLE_PHYSICAL_MEMORY
  )
  cmake_host_system_information(RESULT _${key} QUERY ${key})
endforeach()


#CMakeLists.txt中的foreach循环会查询多个键值，并定义相应的变量。
#此示例的核心函数是cmake_host_system_information，它查询运行CMake的主机系统的系统信息。
#本例中，我们对每个键使用了一个函数调用。然后，使用这些变量来配置config.h.in中的占位符，
#输入并生成config.h。此配置使用configure_file命令完成。
#最后，config.h包含在processor-info.cpp中。
#编译后，它将把值打印到屏幕上。我们将在第5章(配置时和构建时操作)和第6章(生成源代码)中重新讨论这种方法。
configure_file(config.h.in config.h @ONLY)
