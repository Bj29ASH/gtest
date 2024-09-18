cmake_minimum_required(VERSION 3.27)
project(gtest VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(gtest INTERFACE)

target_include_directories(gtest
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/googletest/include>
  $<INSTALL_INTERFACE:include/gtest>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/googlemock/include>
  $<INSTALL_INTERFACE:include/gmock>
)

target_link_directories(gtest
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib>
  $<INSTALL_INTERFACE:lib/gtest>
)

target_link_libraries(gtest
  INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/gmock.lib>
  $<INSTALL_INTERFACE:gmock.lib>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/gmock_main.lib>
  $<INSTALL_INTERFACE:gmock_main.lib>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/gtest.lib>
  $<INSTALL_INTERFACE:gtest.lib>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/lib/gtest_main.lib>
  $<INSTALL_INTERFACE:gtest_main.lib>
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/gtestConfig.cmake.in"
  "${CMAKE_CURRENT_BINARY_DIR}/gtestConfig.cmake"
  INSTALL_DESTINATION share/gtest
)

install(TARGETS gtest
  EXPORT gtestTargets
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY ${CMAKE_SOURCE_DIR}/googletest/include/gtest DESTINATION include/gtest)
install(DIRECTORY ${CMAKE_SOURCE_DIR}/googlemock/include/gmock DESTINATION include/gmock)

file(GLOB GTEST_LIBRARIES ${CMAKE_SOURCE_DIR}/lib/*.lib)
install(FILES ${GTEST_LIBRARIES} DESTINATION lib/gtest)

install(
  EXPORT gtestTargets
  DESTINATION share/gtest
  NAMESPACE gtest::
)

install(
  FILES
  "${CMAKE_CURRENT_BINARY_DIR}/gtestConfig.cmake"
  DESTINATION share/gtest
)