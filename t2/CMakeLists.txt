project(HELLO)
cmake_minimum_required(VERSION 3.11)
# ADD_SUBDIRECTORY 指令
# ADD_SUBDIRECTORY(source_dir [binary_dir] [EXCLUDE_FROM_ALL])
# 这个指令用于向当前工程添加存放源文件的子目录，并可以指定中间二进制和目标二进制存
# 放的位置。EXCLUDE_FROM_ALL 参数的含义是将这个目录从编译过程中排除，比如，工程
# 的 example，可能就需要工程构建完成后，再进入 example 目录单独进行构建(当然，你
# 也可以通过定义依赖来解决此类问题)。
add_subdirectory(src bin)

# 换个地方保存目标二进制
# 不论是 SUBDIRS 还是 ADD_SUBDIRECTORY 指令(不论是否指定编译输出目录)，我们都可
# 以通过 SET 指令重新定义 EXECUTABLE_OUTPUT_PATH 和 LIBRARY_OUTPUT_PATH 变量
# 来指定最终的目标二进制的位置(指最终生成的 hello 或者最终的共享库，不包含编译生成
# 的中间文件)
# SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)
# SET(LIBRARY_OUTPUT_PATH ${PROJECT_BINARY_DIR}/lib)
# 在第一节我们提到了<projectname>_BINARY_DIR 和 PROJECT_BINARY_DIR 变量，他
# 们指的编译发生的当前目录，如果是内部编译，就相当于 PROJECT_SOURCE_DIR 也就是
# 工程代码所在目录，如果是外部编译，指的是外部编译所在目录，也就是本例中的 build
# 目录。

# 问题是，我应该把这两条指令写在工程的 CMakeLists.txt 还是 src 目录下的
# CMakeLists.txt，把握一个简单的原则，在哪里 ADD_EXECUTABLE 或 ADD_LIBRARY，
# 如果需要改变目标存放路径，就在哪里加入上述的定义。

install(PROGRAMS runhello.sh DESTINATION bin)
install(FILES COPYRIGHT README DESTINATION share/doc/cmake/t2)
install(DIRECTORY doc/ DESTINATION share/doc/cmake/t2)