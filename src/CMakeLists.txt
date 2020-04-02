cmake_minimum_required(VERSION 3.17)
include(GNUInstallDirs)

find_package(spdlog REQUIRED)

add_library(Fact SHARED fact.cpp)
target_include_directories(Fact
    PUBLIC
    	$<INSTALL_INTERFACE:include>    
	    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
)
target_link_libraries(Fact spdlog::spdlog)
