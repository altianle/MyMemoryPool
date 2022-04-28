cmake_minimum_required(VERSION 3.22)
project(My_malloc)

set(CMAKE_CXX_STANDARD 14)

add_executable(My_malloc main.cpp ThreadCache.h CentralCache.h Common.h CentralCache.cpp ThreadCache.cpp PageCache.h PageCache.cpp BenchMark.cpp ConcurrentAlloc.h UnitTest.cpp)
find_package(Threads REQUIRED)
target_link_libraries(My_malloc Threads::Threads)
