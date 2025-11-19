cmake_minimum_required(VERSION 3.16)
project(iniconfig VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Configure installation directories
include(GNUInstallDirs)

# Option to build shared or static library
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

# Option to build tests (disable by default when used via FetchContent)
if(CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    # Standalone build
    option(BUILD_TESTING "Build tests" ON)
else()
    # Used as subdirectory (e.g., via FetchContent)
    option(BUILD_TESTING "Build tests" OFF)
endif()

add_subdirectory(src)

# Add tests if enabled
if(BUILD_TESTING)
    enable_testing()
    add_subdirectory(tests)
endif()