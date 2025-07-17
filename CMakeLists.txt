cmake_minimum_required(VERSION 3.12)
project(fastmavlink VERSION 1.0.0 LANGUAGES C CXX)

# Create interface libraries for each dialect
add_library(fastmavlink_asluav INTERFACE)
add_library(fastmavlink_avssuas INTERFACE)
add_library(fastmavlink_ardupilotmega INTERFACE)
add_library(fastmavlink_common INTERFACE)
add_library(fastmavlink_cubepilot INTERFACE)
add_library(fastmavlink_icarous INTERFACE)
add_library(fastmavlink_minimal INTERFACE)
add_library(fastmavlink_python_array_test INTERFACE)
add_library(fastmavlink_storm32 INTERFACE)
add_library(fastmavlink_ualberta INTERFACE)

# Set include directories for each dialect
target_include_directories(fastmavlink_asluav INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/ASLUAV>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_avssuas INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/AVSSUAS>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_ardupilotmega INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/ardupilotmega>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_common INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/common>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_cubepilot INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/cubepilot>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_icarous INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/icarous>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_minimal INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/minimal>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_python_array_test INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/python_array_test>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_storm32 INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/storm32>
    $<INSTALL_INTERFACE:include>
)

target_include_directories(fastmavlink_ualberta INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/lib>
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/c_library/ualberta>
    $<INSTALL_INTERFACE:include>
)

# Create aliases for easier usage
add_library(fastmavlink::asluav ALIAS fastmavlink_asluav)
add_library(fastmavlink::avssuas ALIAS fastmavlink_avssuas)
add_library(fastmavlink::ardupilotmega ALIAS fastmavlink_ardupilotmega)
add_library(fastmavlink::common ALIAS fastmavlink_common)
add_library(fastmavlink::cubepilot ALIAS fastmavlink_cubepilot)
add_library(fastmavlink::icarous ALIAS fastmavlink_icarous)
add_library(fastmavlink::minimal ALIAS fastmavlink_minimal)
add_library(fastmavlink::python_array_test ALIAS fastmavlink_python_array_test)
add_library(fastmavlink::storm32 ALIAS fastmavlink_storm32)
add_library(fastmavlink::ualberta ALIAS fastmavlink_ualberta)

# Optional: Install configuration
include(GNUInstallDirs)

install(TARGETS fastmavlink_asluav fastmavlink_avssuas fastmavlink_ardupilotmega fastmavlink_common fastmavlink_cubepilot fastmavlink_icarous fastmavlink_minimal fastmavlink_python_array_test fastmavlink_storm32 fastmavlink_ualberta
    EXPORT fastmavlinkTargets
)

install(DIRECTORY c_library/lib/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/lib)
install(DIRECTORY c_library/ASLUAV/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/ASLUAV)
install(DIRECTORY c_library/AVSSUAS/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/AVSSUAS)
install(DIRECTORY c_library/ardupilotmega/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/ardupilotmega)
install(DIRECTORY c_library/common/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/common)
install(DIRECTORY c_library/cubepilot/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/cubepilot)
install(DIRECTORY c_library/icarous/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/icarous)
install(DIRECTORY c_library/minimal/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/minimal)
install(DIRECTORY c_library/python_array_test/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/python_array_test)
install(DIRECTORY c_library/storm32/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/storm32)
install(DIRECTORY c_library/ualberta/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/fastmavlink/ualberta)

install(EXPORT fastmavlinkTargets
    FILE fastmavlinkTargets.cmake
    NAMESPACE fastmavlink::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/fastmavlink
)

# Create config file
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    fastmavlinkConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/fastmavlinkConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/fastmavlinkConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/fastmavlink
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/fastmavlinkConfig.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/fastmavlinkConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/fastmavlink
)
