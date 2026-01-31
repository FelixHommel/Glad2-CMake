cmake_minimum_required(VERSION 3.30)
project(
    glad2cmake
    VERSION 1.1.7
    DESCRIPTION "Multi-Langugae GL Loader-Generator."
    LANGUAGES C CXX
)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(${PROJECT_NAME} ./src/gl.c)
if(WIN32 AND BUILD_SHARED_LIBS)
    set_target_properties(${PROJECT_NAME} PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()
target_sources(${PROJECT_NAME}
    PRIVATE
        FILE_SET HEADERS
        FILES
            ./include/glad/gl.h
            ./include/KHR/khrplatform.h
)
add_library(Glad2CMake::Glad2CMake ALIAS glad2cmake)
target_include_directories(${PROJECT_NAME}
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

include(GNUInstallDirs)

install(
    TARGETS glad2cmake
    EXPORT glad2cmakeTargets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

install(
    EXPORT glad2cmakeTargets
    FILE glad2cmakeTargets.cmake
    NAMESPACE Glad2CMake::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/glad2cmake
)

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/glad2cmakeConfigVersion.cmake"
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/glad2cmakeConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/glad2cmakeConfig.cmake"
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/glad2cmake
)

install(
    FILES
        "${CMAKE_CURRENT_BINARY_DIR}/glad2cmakeConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/glad2cmakeConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/glad2cmake
)
