# set minimum cmake version
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

# project name and language
project(recipe-10 LANGUAGES CXX)

add_library(geometry
  STATIC
    geometry_circle.cpp
    geometry_circle.hpp
    geometry_polygon.cpp
    geometry_polygon.hpp
    geometry_rhombus.cpp
    geometry_rhombus.hpp
    geometry_square.cpp
    geometry_square.hpp
  )

# we wish to compile the library with the optimization flag: -O3
target_compile_options(geometry
  PRIVATE
    -O3
  )

list(
  APPEND filelist
    geometry_circle.cpp
    geometry_rhombus.cpp
  )


#set_source_files_properties(file PROPERTIES property value)，它将属性设置为给定文件的传递值。
#    与目标非常相似，文件在CMake中也有属性，允许对构建系统进行非常细粒度的控制。源文件的可用属性列表可以在这里找到: https://cmake.org/cmake/help/v3.5/manual/cmake-properties.7.html#source-file-properties 。
#get_source_file_property(VAR file property)，检索给定文件所需属性的值，并将其存储在CMakeVAR变量中。


#NOTE:CMake中，列表是用分号分隔的字符串组。列表可以由list或set命令创建。例如，set(var a b c d e)和list(APPEND a b c d e)都创建了列表a;b;c;d;e

# we use the IN LISTS foreach syntax to set source properties
message(STATUS "Setting source properties using IN LISTS syntax:")
foreach(thissource IN LISTS filelist)
  set_source_files_properties(${thissource} PROPERTIES COMPILE_FLAGS -O2)
  message(STATUS "Appending -O2 flag for ${_source}")
endforeach()

# we demonstrate the plain foreach syntax to query source properties
# which requires to expand the contents of the variable
#为了确保设置属性，再次循环并在打印每个源文件的COMPILE_FLAGS
message(STATUS "Querying sources properties using plain syntax:")
foreach(_source ${filelist})
  get_source_file_property(_flags ${_source} COMPILE_FLAGS)
  message(STATUS "Source ${_source} has the following extra COMPILE_FLAGS: ${_flags}")
endforeach()

add_executable(compute-areas compute-areas.cpp)

target_link_libraries(compute-areas geometry)
