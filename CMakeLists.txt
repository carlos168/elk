# CMake 最低版本号要求
cmake_minimum_required(VERSION 2.8)

# 项目信息
project(err_code_test)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_FLAGS "-Wall -g -D_GNU_SOURCE")
include_directories(/usr/local/include)
link_directories(/usr/local/lib)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

# 指定生成目标
add_executable(err_code_test ${DIR_SRCS})
target_link_libraries(err_code_test zlog pthread)


#cmake_minimum_required(VERSION 2.8)
#project(st)
#set(CMAKE_C_STANDARD 99) 
#set(CMAKE_C_FLAGS "-Wall -g -D_GNU_SOURCE")
#set(SOURCE_FILES main.c server.c server.h uthash.h utlist.h cJSON.c cJSON.h forward.c forward.h stats.c stats.h push.c push.h watch.c watch.h)
#include_directories(/usr/include)
#link_directories(/usr/lib)
#add_executable(st ${SOURCE_FILES})
#target_link_libraries(st uv zlog)

