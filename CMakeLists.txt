cmake_minimum_required(VERSION 3.17)

project(Fact VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 14)
option(ENABLE_TESTING "Build test programs" ON)

add_subdirectory(src)

if(ENABLE_TESTING)
    enable_testing()
    add_subdirectory(test)
endif()
