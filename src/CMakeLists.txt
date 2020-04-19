cmake_minimum_required(VERSION 3.17)
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

find_package(spdlog REQUIRED)

add_library(Fact fact.cpp)
target_include_directories(Fact
    PUBLIC
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
)
target_link_libraries(Fact spdlog::spdlog)

if (ENABLE_CPPCHECK)
    find_program(CMAKE_CXX_CPPCHECK NAMES cppcheck REQUIRED)
    list(
        APPEND CMAKE_CXX_CPPCHECK
            "--enable=all"
            "--inconclusive"
            "--force" 
            "--inline-suppr"
            "--suppressions-list=${CMAKE_SOURCE_DIR}/CppCheckSuppressions.txt"
    )
endif()


# Install
set(FACT_INSTALL_DIR ${CMAKE_INSTALL_LIBDIR}/cmake/Fact)

install(TARGETS Fact
    EXPORT fact-export
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/
    DESTINATION
        ${CMAKE_INSTALL_INCLUDEDIR}
)

install(EXPORT fact-export
    FILE
        FactTargets.cmake
    NAMESPACE
        Fact::
    DESTINATION
        ${FACT_INSTALL_DIR}
)

configure_package_config_file(
    ${PROJECT_SOURCE_DIR}/cmake/FactConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/FactConfig.cmake
    INSTALL_DESTINATION ${FACT_INSTALL_DIR}
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/FactConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/FactConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/FactConfigVersion.cmake
    DESTINATION
        ${FACT_INSTALL_DIR}
)

export(EXPORT fact-export
    FILE
        ${PROJECT_BINARY_DIR}/FactTargets.cmake
    NAMESPACE Fact::
)

export(PACKAGE Fact)
