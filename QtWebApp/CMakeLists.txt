cmake_minimum_required(VERSION 3.20)

# QtWebApp Library
project(QtWebApp VERSION 1.9.0 LANGUAGES CXX)

# Find Qt
find_package(Qt6 REQUIRED COMPONENTS Core Network)

# Try to find Core5Compat - it's optional
find_package(Qt6 QUIET COMPONENTS Core5Compat)

# Create the library
add_library(QtWebApp SHARED)

# Create alias for consistent usage
add_library(QtWebApp::QtWebApp ALIAS QtWebApp)

# Set target properties
set_target_properties(QtWebApp PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED ON
)

# Link Qt libraries
target_link_libraries(QtWebApp PUBLIC
    Qt6::Core
    Qt6::Network
)

# Platform-specific settings
if(WIN32)
    # Windows specific defines for shared library export
    target_compile_definitions(QtWebApp PRIVATE QTWEBAPPLIB_EXPORT)

    # Debug suffix for Windows
    set_target_properties(QtWebApp PROPERTIES
        DEBUG_POSTFIX "d"
    )
elseif(APPLE)
    # macOS specific settings
    set_target_properties(QtWebApp PROPERTIES
        DEBUG_POSTFIX "_debug"
        INSTALL_NAME_DIR "/usr/local/lib/"
    )
else()
    # Unix/Linux debug suffix
    set_target_properties(QtWebApp PROPERTIES
        DEBUG_POSTFIX "d"
    )
endif()

# Include subdirectories
add_subdirectory(logging)
add_subdirectory(httpserver)
add_subdirectory(templateengine)

# Set up include directories for the library
target_include_directories(QtWebApp PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/logging>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/httpserver>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/templateengine>
    $<INSTALL_INTERFACE:include/QtWebApp>
)

# Export the library for use by other projects
set_target_properties(QtWebApp PROPERTIES EXPORT_NAME QtWebApp)

# Installation rules
include(GNUInstallDirs)

install(TARGETS QtWebApp
    EXPORT QtWebAppTargets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# Install headers
install(DIRECTORY logging/ httpserver/ templateengine/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/QtWebApp
    FILES_MATCHING PATTERN "*.h"
)

# Export targets
install(EXPORT QtWebAppTargets
    FILE QtWebAppTargets.cmake
    NAMESPACE QtWebApp::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/QtWebApp
)

# Create config file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    QtWebAppConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/QtWebAppConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/QtWebAppConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/QtWebApp
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/QtWebAppConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/QtWebAppConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/QtWebApp
)