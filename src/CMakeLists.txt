cmake_minimum_required(VERSION 3.17)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

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

configure_package_config_file(
	${PROJECT_SOURCE_DIR}/cmake/FactConfig.cmake.in
	${CMAKE_CURRENT_BINARY_DIR}/FactConfig.cmake
	INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/Fact/cmake
)

write_basic_package_version_file(
	${CMAKE_CURRENT_BINARY_DIR}/FactConfigVersion.cmake
	VERSION ${PROJECT_VERSION}
	COMPATIBILITY SameMajorVersion
)

install(
	FILES
		${CMAKE_CURRENT_BINARY_DIR}/FactConfig.cmake
		${CMAKE_CURRENT_BINARY_DIR}/FactConfigVersion.cmake
		${CMAKE_BINARY_DIR}/FactTargets.cmake
	DESTINATION ${CMAKE_INSTALL_LIBDIR}/Fact/cmake
)

install(TARGETS Fact DESTINATION "${CMAKE_INSTALL_LIBDIR}/Fact/cmake")

export(TARGETS Fact
	FILE "${PROJECT_BINARY_DIR}/FactTargets.cmake"
	NAMESPACE Fact::
)

export(PACKAGE Fact)
