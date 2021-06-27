cmake_minimum_required(VERSION 2.6)


project(mycmaketest)

#如果将逻辑变量设置为以下任意一种：1、ON、YES、true、Y或非零数，则逻辑变量为true。
#如果将逻辑变量设置为以下任意一种：0、OFF、NO、false、N、IGNORE、NOTFOUND、空字符串，或者以-NOTFOUND为后缀，则逻辑变量为false

#添加变量 设置为OFF
set(USE_LIBRARY ON)
message(STATUS "Compile sources into a library? ${USE_LIBRARY}")
message("-----------USE_LIBRARY ${USE_LIBRARY}")

message("-----------${BUILD_SHARED_LIBS}")
set(BUILD_SHARED_LIBS Y)
message("-----------${BUILD_SHARED_LIBS}")

list(APPEND _sources Message.hpp Message.cpp)

message("-----------${_sources}")




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
