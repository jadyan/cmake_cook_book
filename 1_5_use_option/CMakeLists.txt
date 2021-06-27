cmake_minimum_required(VERSION 2.6)

include(CMakeDependentOption)

project(mycmaketest)



#option可接受三个参数：
#
#option(<option_variable> "help string" [initial value])
#	<option_variable>表示该选项的变量的名称。
#	"help string"记录选项的字符串，在CMake的终端或图形用户界面中可见。
#	[initial value]选项的默认值，可以是ON或OFF。

#必须三个参数 不然不起作用（USE_LIBRARY 始终不是ON）
option(USE_LIBRARY "option set" ON)

message(STATUS "|||Compile sources into a library? ${USE_LIBRARY}")
message("-----------USE_LIBRARY ${USE_LIBRARY}")

message("-----------${BUILD_SHARED_LIBS}")
set(BUILD_SHARED_LIBS Y)
message("-----------${BUILD_SHARED_LIBS}")

list(APPEND _sources Message.hpp Message.cpp)

message("-----------${_sources}")


#CMake提供cmake_dependent_option()命令用来定义依赖于其他选项的选项
#如果USE_LIBRARY为ON，MAKE_STATIC_LIBRARY默认值为OFF，
# second option depends on the value of the first
cmake_dependent_option(
    MAKE_STATIC_LIBRARY "Compile sources into a static library" OFF
    "USE_LIBRARY" ON
    )


#如果USE_LIBRARY为ON，MAKE_SHARED_LIBRARY默认值为ON
# third option depends on the value of the first
cmake_dependent_option(
    MAKE_SHARED_LIBRARY "Compile sources into a shared library" ON
    "USE_LIBRARY" ON
    )

message("-------------MAKE_STATIC_LIBRARY:${MAKE_STATIC_LIBRARY}")
message("-------------MAKE_SHARED_LIBRARY:${MAKE_SHARED_LIBRARY}")





#add_library(message STATIC Message.hpp Message.cpp)：生成必要的构建指令，将指定的源码编译到库中。add_library的第一个参数是目标名。整个CMakeLists.txt中，可使用相同的名称来引用库。
#生成的库的实际名称将由CMake通过在前面添加前缀lib和适当的扩展名作为后缀来形成。生成库是根据第二个参数(STATIC或SHARED)和操作系统确定的。
#target_link_libraries(hello-world message): 将库链接到可执行文件。此命令还确保hello-world可执行文件可以正确地依赖于消息库。因此，在消息库链接到hello-world可执行文件之前，需要完成消息库的构建。

if(USE_LIBRARY)
    # add_library will create a static library
    # since BUILD_SHARED_LIBS is OFF

    #创建目标——静态库  生成的时候会以名字message 加头加尾 libmessage.a
    add_library(message  SHARED ${_sources})
    add_executable(hello-world hello-world.cpp)

    #将目标库链接到可执行目标
    target_link_libraries(hello-world message)
else()
    add_executable(hello-world hello-world.cpp ${_sources})
endif()
