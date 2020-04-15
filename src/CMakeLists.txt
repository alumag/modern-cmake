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

find_program(CMAKE_CXX_CPPCHECK NAMES cppcheck)
if (ENABLE_CPPCHECK AND CMAKE_CXX_CPPCHECK)
    list(
        APPEND CMAKE_CXX_CPPCHECK 
            "--enable=warning"
            "--inconclusive"
            "--force" 
            "--inline-suppr"
            "--suppressions-list=${CMAKE_SOURCE_DIR}/CppCheckSuppressions.txt"
    )

endif()

export(TARGETS Fact
	FILE "${PROJECT_BINARY_DIR}/FactTargets.cmake"
	NAMESPACE Fact::
)

export(PACKAGE Fact)

install(
	FILES "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/FactConfig.cmake"
	DESTINATION "${LIB_INSTALL_DIR}/Fact/cmake"
)

install(TARGETS Fact DESTINATION "${LIB_INSTALL_DIR}/Fact/cmake")
