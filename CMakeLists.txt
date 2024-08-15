cmake_minimum_required(VERSION 3.10)

project(libipc++)


option(BUILD_RELEASE_LIB "Build release lib" ON)

if(BUILD_RELEASE_LIB)
set(CMAKE_CXX_FLAGS_RELEASE "-O2 -DNDEBUG")
else()
set(CMAKE_CXX_FLAGS_DEBUG "-g -DDEBUG_BUILD")
endif()

if(NOT BUILD_RELEASE_LIB)
  find_package(benchmark REQUIRED)
endif()

set(CMAKE_CXX_STANDARD 14)

include_directories(include)
list(APPEND libs rt pthread stdc++fs)
add_compile_options(-march=native)
IF(SINGLE_H)
  add_definitions(-DSINGLE_HEADER)
ENDIF(SINGLE_H)


# TESTS
add_executable(pubsub_test test/pubsub_test.cpp)
target_link_libraries(pubsub_test ${libs})

add_executable(rpc_test test/rpc_test.cpp)
target_link_libraries(rpc_test ${libs})

add_executable(dragons_test test/dragons_test.cpp)
target_link_libraries(dragons_test ${libs})

add_executable(allocator_test test/allocator_test.cpp)
target_link_libraries(allocator_test ${libs})

if(NOT BUILD_RELEASE_LIB)
# BENCHMARKS
add_executable(dragons_bench benchmark/dragons.cpp)
target_link_libraries(dragons_bench ${libs} benchmark::benchmark)

add_executable(pubsub_bench benchmark/pubsub.cpp)
target_link_libraries(pubsub_bench ${libs})

add_executable(rpc_bench benchmark/rpc.cpp)
target_link_libraries(rpc_bench ${libs})
endif()
