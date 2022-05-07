#cmake最小版本
cmake_minimum_required(VERSION 2.8) 
#指定项目
project(sylar)

# 相当于一个复制操作，将CMAKE_VERBOSE_MAKEFILE的值设为ON
set(CMAKE_VERBOSE_MAKEFILE ON)
# set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -rdynamic -O0 -ggdb -std=c++11 -Wall -Wno-deprecated -Werror -Wno-unused-function")
# set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -O0 -ggdb -std=c++11 -Wall -Wno-deprecated -Werror -Wno-unused-function")
set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -std=c++11")

# include_directories ([AFTER|BEFORE] [SYSTEM] dir1 [dir2 ...])
# 将指定目录添加到编译器的头文件搜索路径之下，指定的目录被解释成当前源码路径的相对路径。
include_directories(.)
# include_directories(/include)
link_directories(/lib)

# find_library命令用于查找库（动态库或者静态库），当构建依赖于第三方库/系统库，可以使用该命令来查找并使用库
# set(YAMLCPP ${PROJECT_SOURCE_DIR}/lib/libsylar.dylib)
# message("**--*", ${YAMLCPP})

set(LIB_SRC
    sylar/log.cc
    sylar/util.cc
    # sylar/config.cc
    )

# 将指定的源⽂件⽣成链接⽂件，然后添加到⼯程中去
# add_library(<name> [STATIC | SHARED | MODULE]
# [EXCLUDE_FROM_ALL]
# [source1] [source2] [...])
add_library(sylar SHARED ${LIB_SRC})
add_library(sylar_static STATIC ${LIB_SRC})
SET_TARGET_PROPERTIES (sylar_static PROPERTIES OUTPUT_NAME "sylar")

# 构建可执行文件
add_executable(test tests/test.cc)
# 生成可执行文件test之前，需要先生成sylar，因为sylar是test的依赖
add_dependencies(test sylar)
target_link_libraries(test sylar)
# target_link_libraries(test sylar ${YAMLCPP})

# add_executable(test_config tests/test_config.cc)
# add_dependencies(test_config sylar)
# target_link_libraries(test_config sylar ${YAMLCPP})

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)
