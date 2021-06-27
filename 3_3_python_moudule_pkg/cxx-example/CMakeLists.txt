#必须先加环境变量才能用
#export PATH=$PATH:/usr/local/python3/lib/:/usr/local/python3/include/python3.7m/



#首先，我们定义了最低CMake版本、项目名称、语言和C++标准:
# set minimum cmake version
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

# project name and language
project(recipe-03 LANGUAGES CXX)

#c++ 标准
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


#
#PYTHONINTERP_FOUND：是否找到解释器
#PYTHON_EXECUTABLE：Python解释器到可执行文件的路径
#PYTHON_VERSION_STRING：Python解释器的完整版本信息
#PYTHON_VERSION_MAJOR：Python解释器的主要版本号
#PYTHON_VERSION_MINOR ：Python解释器的次要版本号
#PYTHON_VERSION_PATCH：Python解释器的补丁版本号
#可以强制CMake，查找特定版本的包。例如，要求Python解释器的版本大于或等于2.7：find_package(PythonInterp 2.7)

#查找解释器、头文件和库的方法与前面的方法完全相同:
find_package(PythonInterp REQUIRED)
find_package(PythonLibs ${PYTHON_VERSION_MAJOR}.${PYTHON_VERSION_MINOR} EXACT REQUIRED)

	
	
message("----PYTHON_INCLUDE_DIRS = ${PYTHON_INCLUDE_DIRS}")
message("----PYTHON_LIBRARIES = ${PYTHON_LIBRARIES}")



#execute_process将作为通过子进程执行一个或多个命令。最后，子进程返回值将保存到变量作为参数，
#传递给RESULT_VARIABLE，
#而管道标准输出和标准错误的内容将被保存到变量作为参数传递给OUTPUT_VARIABLE和ERROR_VARIABLE。
#execute_process可以执行任何操作，并使用它们的结果来推断系统配置。本例中，用它来确保NumPy可用，然后获得模块版本。

#如果找到NumPy，则_numpy_status变量为整数，否则为错误的字符串，
#而_numpy_location将包含NumPy模块的路径。
# Find NumPy location
execute_process(
  COMMAND
    ${PYTHON_EXECUTABLE} "-c" "import re, numpy; print(re.compile('/__init__.py.*').sub('',numpy.__file__))"
  RESULT_VARIABLE _numpy_status
  OUTPUT_VARIABLE _numpy_location
  ERROR_QUIET
  OUTPUT_STRIP_TRAILING_WHITESPACE
  )

#如果找到NumPy，则将它的位置保存到一个名为NumPy的新变量中。注意，新变量被缓存，这意味着CMake创建了一个持久性变量，用户稍后可以修改该变量:
if(NOT _numpy_status)
  set(NumPy ${_numpy_location} CACHE STRING "Location of NumPy")
  #set(NumPy ${_numpy_location})
endif()

message("-------------------NumPy  ${NumPy}")




# Find NumPy version
#检查numpy的版本
execute_process(
  COMMAND
    ${PYTHON_EXECUTABLE} "-c" "import numpy; print(numpy.__version__)"
  OUTPUT_VARIABLE _numpy_version
  ERROR_QUIET
  OUTPUT_STRIP_TRAILING_WHITESPACE
  )


#find_package_handle_standard_args提供了，用于处理与查找相关程序和库的标准工具。
#引用此命令时，可以正确的处理与版本相关的选项(REQUIRED和EXACT)，而无需更多的CMake代码。
#所有必需的变量都设置为有效的文件路径(NumPy)后，发送到模块(NumPy_FOUND)。它还将版本保存在可传递的版本变

message("NumPy_FOUND = ${NumPy_FOUND}")
include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(NumPy
  FOUND_VAR NumPy_FOUND
  REQUIRED_VARS NumPy
  VERSION_VAR _numpy_version
  )


message("NumPy_FOUND = ${NumPy_FOUND}")

add_executable(pure-embedding "")

target_sources(pure-embedding
  PRIVATE
    Py${PYTHON_VERSION_MAJOR}-pure-embedding.cpp
  )

target_include_directories(pure-embedding
  PRIVATE
    ${PYTHON_INCLUDE_DIRS}
  )

target_link_libraries(pure-embedding
  PRIVATE
    ${PYTHON_LIBRARIES}
  )

add_custom_command(
  OUTPUT
    ${CMAKE_CURRENT_BINARY_DIR}/use_numpy.py
  COMMAND
    ${CMAKE_COMMAND} -E copy_if_different ${CMAKE_CURRENT_SOURCE_DIR}/use_numpy.py
                                          ${CMAKE_CURRENT_BINARY_DIR}/use_numpy.py
  DEPENDS
    ${CMAKE_CURRENT_SOURCE_DIR}/use_numpy.py
  )

# make sure building pure-embedding triggers the above
# custom command
target_sources(pure-embedding
  PRIVATE
    ${CMAKE_CURRENT_BINARY_DIR}/use_numpy.py
  )
