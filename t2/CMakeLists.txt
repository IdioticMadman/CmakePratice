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


# INSTALL指令包含了各种安装类型
# 目标文件的安装，需要在add_Library或者add_executeable进行指定，分为三种类型
# ARCHIVE LIBRARY RUNTIME，格式如下
#  INSTALL(TARGETS targets...
#  [[ARCHIVE|LIBRARY|RUNTIME] [DESTINATION <dir>]
#  [PERMISSIONS permissions...]
#  [CONFIGURATIONS [Debug|Release|...]]
#  [COMPONENT <component>]
#  [OPTIONAL]
#  ] [...])
# eg: install(TARGETS hello RUNTIME DESTINATION bin)

# 非目标文件的可执行程序安装
# INSTALL(PROGRAMS files... DESTINATION <dir>
#             [PERMISSIONS permissions...]
#             [CONFIGURATIONS [Debug|Release|...]]
#             [COMPONENT <component>]
#             [RENAME <name>] [OPTIONAL])
# 默认权限是755

install(PROGRAMS runhello.sh DESTINATION bin)

# 普通文件的安装
# INSTALL(FILES files... DESTINATION <dir>
#             [PERMISSIONS permissions...]
#             [CONFIGURATIONS [Debug|Release|...]]
#             [COMPONENT <component>]
#             [RENAME <name>] [OPTIONAL])
# 默认权限是644
install(FILES COPYRIGHT README DESTINATION share/doc/cmake/t2)

# 文件夹的安装
# INSTALL(DIRECTORY dirs... DESTINATION <dir>
#             [FILE_PERMISSIONS permissions...]
#             [DIRECTORY_PERMISSIONS permissions...]
#             [USE_SOURCE_PERMISSIONS]
#             [CONFIGURATIONS [Debug|Release|...]]
#             [COMPONENT <component>]
#             [[PATTERN <pattern> | REGEX <regex>]
#              [EXCLUDE] [PERMISSIONS permissions...]] [...])
# 如果目录名不以/结尾，那么这个目录将被安装为目标路径下的 abc，如果目录名以/结尾， 
# 代表将这个目录中的内容安装到目标路径，但不包括这个目录本身。
install(DIRECTORY doc/ DESTINATION share/doc/cmake/t2)