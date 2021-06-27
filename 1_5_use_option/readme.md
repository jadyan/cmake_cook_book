可以通过CMake的-DCLI选项，将信息传递给CMake来切换库的行为

$ mkdir -p build
$ cd build
#同样设置USE_LIBRARY为ON
$ cmake -D USE_LIBRARY=ON ..
