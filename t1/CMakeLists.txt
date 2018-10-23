#你可以用这个指令定义工程名称，并可指定工程支持的语言，支持的语言列表是可以忽略的，
#默认情况表示支持所有语言。这个指令隐式的定义了两个 cmake 变量:
#<projectname>_BINARY_DIR 以及<projectname>_SOURCE_DIR，这里就是
#HELLO_BINARY_DIR 和 HELLO_SOURCE_DIR(所以 CMakeLists.txt 中两个 MESSAGE指令可以直接使用了这两个变量)
#为了统一起见，建议以后直接使用 PROJECT_BINARY_DIR，PROJECT_SOURCE_DIR，即
#使修改了工程名称，也不会影响这两个变量。如果使用了<projectname>_SOURCE_DIR，修改工程名称后，需要同时修改这些变量。
PROJECT(HELLO C)

#SET 指令的语法是：
#SET(VAR [VALUE] [CACHE TYPE DOCSTRING [FORCE]])
#现阶段，你只需要了解 SET 指令可以用来显式的定义变量即可。
#比如我们用到的是 SET(SRC_LIST main.c)，如果有多个源文件，也可以定义成：SET(SRC_LIST main.c t1.c t2.c)。
SET(SRC_LIST main.c )

# MESSAGE 指令的语法是：
# MESSAGE([SEND_ERROR | STATUS | FATAL_ERROR] "message to display"...)
# 这个指令用于向终端输出用户定义的信息，包含了三种类型:
# SEND_ERROR，产生错误，生成过程被跳过。
# SATUS，输出前缀为—的信息。FATAL_ERROR，立即终止所有 cmake 过程.
# 我们在这里使用的是 STATUS 信息输出，演示了由 PROJECT 指令定义的两个隐式变量
# HELLO_BINARY_DIR 和 HELLO_SOURCE_DIR。
MESSAGE(STATUS "This is BINARY dir " ${HELLO_BINARY_DIR})
MESSAGE(STATUS "This is SOURCE dir " ${HELLO_SOURCE_DIR})

# 定义了这个工程会生成一个文件名为 hello 的可执行文件，相关的源文件是 SRC_LIST 中
# 定义的源文件列表， 本例中你也可以直接写成 ADD_EXECUTABLE(hello main.c)。
ADD_EXECUTABLE(hello ${SRC_LIST})

# 清理工程
# cmake 并不支持 make distclean，关于这一点，官方是有明确解释的:
# 因为 CMakeLists.txt 可以执行脚本并通过脚本生成一些临时文件，但是却没有办法来跟
# 踪这些临时文件到底是哪些。因此，没有办法提供一个可靠的 make distclean 方案。

# 内部构建与外部构建
# 通过外部编译进行工程构建，HELLO_SOURCE_DIR 仍然指代工程路径，即/backup/cmake/t1
# 而 HELLO_BINARY_DIR 则指代编译路径，即/backup/cmake/t1/build