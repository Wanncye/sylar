cmake_minimum_required(VERSION 2.8) 
project(sylar)

set(CMAKE_VERBOSE_MAKEFILE ON)
set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} -O0 -ggdb -std=c++11 -Wall -Wno-deprecated -Werror -Wno-unused-function")
# yaml安装之后的include路径
include_directories(/usr/local/include) 
include_directories(.)
link_directories(/usr/local/lib)

find_library(YAMLCPP yaml-cpp)
find_library(PTHREAD pthread)
message("[My tag -*-*-*-*]:" ${YAMLCPP})

set(LIB_SRC
    sylar/config.cc
    sylar/fiber.cc
    sylar/log.cc
    sylar/scheduler.cc
    sylar/thread.cc
    sylar/util.cc
    )

set(LIBS
    sylar
    pthread
    yaml-cpp
)

message("[My tag -*-*-*-*]:" ${LIB_SRC})

add_library(sylar SHARED ${LIB_SRC})
# add_library(sylar_static STATIC ${LIB_SRC})
# SET_TARGET_PROPERTIES (sylar_static PROPERTIES OUTPUT_NAME "sylar")

# target_link_libraries(sylar ${YAMLCPP} ${PTHREAD})
target_link_libraries(sylar ${YAMLCPP})

add_executable(test tests/test.cc)
add_dependencies(test sylar)
# target_link_libraries(test sylar ${YAMLCPP})
target_link_libraries(test ${LIBS})

add_executable(test_config tests/test_config.cc)
add_dependencies(test_config sylar)
target_link_libraries(test_config ${LIBS})

add_executable(test_thread tests/test_thread.cc)
add_dependencies(test_thread sylar)
target_link_libraries(test_thread ${LIBS})

add_executable(test_util tests/test_util.cc)
add_dependencies(test_util sylar)
target_link_libraries(test_util ${LIBS})

add_executable(test_fiber tests/test_fiber.cc)
add_dependencies(test_fiber sylar)
target_link_libraries(test_fiber ${LIBS})

add_executable(test_scheduler tests/test_scheduler.cc)
add_dependencies(test_scheduler sylar)
target_link_libraries(test_scheduler ${LIBS})

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
SET(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)