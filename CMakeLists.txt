cmake_minimum_required(VERSION 3.12)
project(Glad)

add_library(glad include/glad/glad.h src/glad.c)
target_include_directories(glad PUBLIC include/)

# Generate the config file for find_package
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/gladConfigVersion.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/gladConfig.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/gladConfig.cmake"
  INSTALL_DESTINATION lib/cmake/glad
)

# Installation rules
install(TARGETS glad
        EXPORT gladTargets
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib)

install(DIRECTORY include/
        DESTINATION include
        FILES_MATCHING PATTERN "*.h")

install(EXPORT gladTargets
        FILE gladTargets.cmake
        NAMESPACE glad::
        DESTINATION lib/cmake/glad)

install(
  FILES "${CMAKE_CURRENT_BINARY_DIR}/gladConfig.cmake"
        "${CMAKE_CURRENT_BINARY_DIR}/gladConfigVersion.cmake"
  DESTINATION lib/cmake/glad
)

