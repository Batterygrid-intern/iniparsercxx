# Create library target (STATIC or SHARED based on BUILD_SHARED_LIBS)
add_library(iniparsercxx iniparsercxx.cpp)

# Add namespaced alias for consistent usage
add_library(iniparsercxx::iniparsercxx ALIAS iniparsercxx)

# Set target properties
set_target_properties(iniparsercxx PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION ${PROJECT_VERSION_MAJOR}
    PUBLIC_HEADER ${PROJECT_SOURCE_DIR}/include/iniparsercxx.hpp
)

# Configure include directories
target_include_directories(iniparsercxx
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

# Install library, headers, and export targets
install(TARGETS iniparsercxx
    EXPORT iniparsercxxTargets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# Install export targets
install(EXPORT iniparsercxxTargets
    FILE iniparsercxxTargets.cmake
    NAMESPACE iniparsercxx::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/iniparsercxx
)

# Create and install package config files
include(CMakePackageConfigHelpers)

configure_package_config_file(
    ${PROJECT_SOURCE_DIR}/cmake/iniparsercxxConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/iniparsercxxConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/iniparsercxx
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/iniparsercxxConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/iniparsercxxConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/iniparsercxxConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/iniparsercxx
)