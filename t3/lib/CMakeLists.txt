set(LIBHELLO_SRC hello.c)
# 因为动态库和静态库的名字不能取相同的，所以这里我们对静态库的target的名字(propreties)进行了修改
add_library(hello SHARED ${LIBHELLO_SRC})
add_library(hello_static STATIC ${LIBHELLO_SRC})

# SET_TARGET_PROPERTIES(target1 target2 ...PROPERTIES prop1 value1prop2 value2 ...)
# 这条指令可以用来设置输出的名称，对于动态库，还可以用来指定动态库版本和API版本。

set_target_properties(hello_static PROPERTIES OUTPUT_NAME hello)
# 对应有个获取属性的指令 VAR->定义的变量名字
# GET_TARGET_PROPERTY(VAR target property)
# 
get_target_property(OUTPUT_VALUE hello_static OUTPUT_NAME)
message(STATUS "This is hello_static OUTPUT_NAME: ${OUTPUT_VALUE}")
# 防止，在构建静态库和动态库的时候，他们的名字一定会清除掉之前的生成文件
set_target_properties(hello PROPERTIES CLEAN_DIRECT_OUTPUT 1)
set_target_properties(hello_static PROPERTIES CLEAN_DIRECT_OUTPUT 1)

# 给动态库设置版本号
set_target_properties(hello PROPERTIES VERSION 1.2 SOVERSION 1)

install(TARGETS hello hello_static
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib)
