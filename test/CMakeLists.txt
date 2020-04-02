cmake_minimum_required(VERSION 3.17)

find_package(CppUTest REQUIRED)

add_executable(FactTests tests.cpp)
target_link_libraries(FactTests PUBLIC CppUTest Fact)

add_test(FactTests FactTests)
