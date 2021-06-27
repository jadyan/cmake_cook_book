cmake_minimum_required(VERSION 2.6)

project(recipe-03 LANGUAGES CXX)


#OBJECT：可将给定add_library的列表中的源码编译到目标文件，不将它们归档到静态库中，也不能将它们链接到共享对象中。如果需要一次性创建静态库和动态库
add_library(message-objs
    OBJECT
        Message.hpp
        Message.cpp
    )

# this is only needed for older compilers
# but doesn't hurt either to have it
#保证编译的目标文件与生成位置无关。可以通过使用set_target_properties命令，设置message-objs目标的相应属性来实现。
set_target_properties(message-objs
    PROPERTIES
        POSITION_INDEPENDENT_CODE 1
    )

add_library(message-shared
  SHARED
    $<TARGET_OBJECTS:message-objs>
    )

#设置目标生成属性
set_target_properties(message-shared
    PROPERTIES
        OUTPUT_NAME "message"
    )
add_library(message-static
    STATIC
        $<TARGET_OBJECTS:message-objs>
    )
set_target_properties(message-static
    PROPERTIES
        OUTPUT_NAME "message"
    )

add_executable(hello-world hello-world.cpp)
target_link_libraries(hello-world message-static)
