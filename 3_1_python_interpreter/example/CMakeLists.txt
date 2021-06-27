# set minimum cmake version
#定义CMake最低版本和项目名称。
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

# project name and language
#注意，这里不需要任何语言支持:
project(recipe-01 LANGUAGES NONE)


message("----PYTHON_EXECUTABLE = ${PYTHON_EXECUTABLE}")


# detect python
#使用find_package命令找到Python解释器:
find_package(PythonInterp REQUIRED)

message("----PYTHON_EXECUTABLE = ${PYTHON_EXECUTABLE}")
message("----PYTHON_VERSION_STRING = ${PYTHON_VERSION_STRING}/${PYTHON_VERSION_MAJOR}/${PYTHON_VERSION_MINOR}/${PYTHON_VERSION_PATCH}")

# Execute a tiny Python script
#执行Python命令并捕获它的输出和返回值:
execute_process(
  COMMAND
    ${PYTHON_EXECUTABLE} "-c" "print('Hello, world!')"
  RESULT_VARIABLE _status
  OUTPUT_VARIABLE _hello_world
  ERROR_QUIET
  OUTPUT_STRIP_TRAILING_WHITESPACE
  )

#最后，打印Python命令的返回值和输出:
message(STATUS "RESULT_VARIABLE is: ${_status}")
message(STATUS "OUTPUT_VARIABLE is: ${_hello_world}")

# compare the "manual" messages with the following handy helper
#调试信息
include(CMakePrintHelpers)
cmake_print_variables(_status _hello_world)




#find_package是用于发现和设置包的CMake模块的命令。这些模块包含CMake命令，用于标识系统标准位置中的包。CMake模块文件称为Find<name>.cmake，当调用find_package(<name>)时，模块中的命令将会运行。
#
#除了在系统上实际查找包模块之外，查找模块还会设置了一些有用的变量，反映实际找到了什么，也可以在自己的CMakeLists.txt中使用这些变量。对于Python解释器，相关模块为FindPythonInterp.cmake附带的设置了一些CMake变量:
#
#PYTHONINTERP_FOUND：是否找到解释器
#PYTHON_EXECUTABLE：Python解释器到可执行文件的路径
#PYTHON_VERSION_STRING：Python解释器的完整版本信息
#PYTHON_VERSION_MAJOR：Python解释器的主要版本号
#PYTHON_VERSION_MINOR ：Python解释器的次要版本号
#PYTHON_VERSION_PATCH：Python解释器的补丁版本号
#可以强制CMake，查找特定版本的包。例如，要求Python解释器的版本大于或等于2.7：find_package(PythonInterp 2.7)