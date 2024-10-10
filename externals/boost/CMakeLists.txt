cmake_minimum_required(VERSION 3.7)

project(boost LANGUAGES CXX VERSION 1.86)

set(CMAKE_CXX_STANDARD 14)

add_library(boost INTERFACE)
target_include_directories(boost INTERFACE
    $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${PROJECT_BINARY_DIR}/boostConfigVersion.cmake"
    VERSION ${BOOST_VERSION}
    COMPATIBILITY AnyNewerVersion
)

install(TARGETS boost
    EXPORT boostTargets
    LIBRARY DESTINATION lib COMPONENT Runtime
    ARCHIVE DESTINATION lib COMPONENT Development
    RUNTIME DESTINATION bin COMPONENT Runtime
    PUBLIC_HEADER DESTINATION include COMPONENT Development
    BUNDLE DESTINATION bin COMPONENT Runtime
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    "${PROJECT_SOURCE_DIR}/boostConfig.cmake.in"
    "${PROJECT_BINARY_DIR}/boostConfig.cmake"
    INSTALL_DESTINATION lib/cmake/fake_boost
)

install(EXPORT boostTargets DESTINATION lib/cmake/fake_boost)
install(FILES "${PROJECT_BINARY_DIR}/boostConfigVersion.cmake"
              "${PROJECT_BINARY_DIR}/boostConfig.cmake"
        DESTINATION lib/cmake/fake_boost)
install(DIRECTORY ${PROJECT_SOURCE_DIR}/include/ DESTINATION include)
