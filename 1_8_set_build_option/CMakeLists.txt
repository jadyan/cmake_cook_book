#$ cd build
#$ cmake ..
#$ cmake --build . -- VERBOSE=1




# set minimum cmake version
#设置CMake的最低版本:
cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

# project name and language
#声明项目名称和语言:
project(recipe-08 LANGUAGES CXX)

#然后，打印当前编译器标志。CMake将对所有C++目标使用这些:
message("C++ compiler flags: ${CMAKE_CXX_FLAGS}")


message("VERBOSE = ${VERBOSE}")
set(VERBOSE "1")
message("VERBOSE = ${VERBOSE}")

#为目标准备了标志列表，其中一些将无法在Windows上使用:
list(APPEND flags "-fPIC" "-Wall")
if(NOT WIN32)
  list(APPEND flags "-Wextra" "-Wpedantic")
endif()

message("--------- ${flags}")


#添加了一个新的目标——geometry库，并列出它的源依赖关系
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


#PRIVATE，编译选项会应用于给定的目标，不会传递给与目标相关的目标。我们的示例中， 即使compute-areas将链接到geometry库，compute-areas也不会继承geometry目标上设置的编译器选项。
#INTERFACE，给定的编译选项将只应用于指定目标，并传递给与目标相关的目标。
#PUBLIC，编译选项将应用于指定目标和使用它的目标。

#目标属性的可见性CMake的核心，我们将在本书中经常讨论这个话题。以这种方式添加编译选项，不会影响全局CMake变量CMAKE_<LANG>_FLAGS_<CONFIG>，并能更细粒度控制在哪些目标上使用哪些选项。


#为这个库目标设置了编译选项:
target_compile_options(geometry
  PRIVATE
    ${flags}
  )

#然后，将生成compute-areas可执行文件作为一个目标:
add_executable(compute-areas compute-areas.cpp)


#还为可执行目标设置了编译选项
target_compile_options(compute-areas
  PRIVATE
    "-fPIC"
  )

#将可执行文件链接到geometry库:
target_link_libraries(compute-areas geometry)





#if(CMAKE_CXX_COMPILER_ID MATCHES GNU)
#  list(APPEND CMAKE_CXX_FLAGS "-fno-rtti" "-fno-exceptions")
#  list(APPEND CMAKE_CXX_FLAGS_DEBUG "-Wsuggest-final-types" "-Wsuggest-final-methods" "-Wsuggest-override")
#  list(APPEND CMAKE_CXX_FLAGS_RELEASE "-O3" "-Wno-unused")
#endif()
#if(CMAKE_CXX_COMPILER_ID MATCHES Clang)
#  list(APPEND CMAKE_CXX_FLAGS "-fno-rtti" "-fno-exceptions" "-Qunused-arguments" "-fcolor-diagnostics")
#  list(APPEND CMAKE_CXX_FLAGS_DEBUG "-Wdocumentation")
#  list(APPEND CMAKE_CXX_FLAGS_RELEASE "-O3" "-Wno-unused")
#endif()




#set(COMPILER_FLAGS)
#set(COMPILER_FLAGS_DEBUG)
#set(COMPILER_FLAGS_RELEASE)
#if(CMAKE_CXX_COMPILER_ID MATCHES GNU)
#  list(APPEND CXX_FLAGS "-fno-rtti" "-fno-exceptions")
#  list(APPEND CXX_FLAGS_DEBUG "-Wsuggest-final-types" "-Wsuggest-final-methods" "-Wsuggest-override")
#  list(APPEND CXX_FLAGS_RELEASE "-O3" "-Wno-unused")
#endif()
#if(CMAKE_CXX_COMPILER_ID MATCHES Clang)
#  list(APPEND CXX_FLAGS "-fno-rtti" "-fno-exceptions" "-Qunused-arguments" "-fcolor-diagnostics")
#  list(APPEND CXX_FLAGS_DEBUG "-Wdocumentation")
#  list(APPEND CXX_FLAGS_RELEASE "-O3" "-Wno-unused")
#endif()

#target_compile_option(compute-areas
#  PRIVATE
#    ${CXX_FLAGS}
#    "$<$<CONFIG:Debug>:${CXX_FLAGS_DEBUG}>"
#    "$<$<CONFIG:Release>:${CXX_FLAGS_RELEASE}>"
#  )