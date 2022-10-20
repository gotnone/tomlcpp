cmake_minimum_required(VERSION 3.13.5)
project(tomlcpp LANGUAGES C CXX)

add_library(tomlcpp tomlcpp.cpp toml.c)
target_include_directories(tomlcpp PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

if(CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
  # We're in the root, define additional targets for developers.
  option(TOMLCPP_BUILD_EXAMPLES "whether or not examples should be built" ON)

  if(TOMLCPP_BUILD_EXAMPLES)
    add_executable(toml_json toml_json.cpp)
    target_link_libraries(toml_json tomlcpp)

    add_executable(toml_sample toml_sample.cpp)
    target_link_libraries(toml_sample tomlcpp)
    add_custom_command(
      TARGET toml_sample
      PRE_BUILD
      COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_SOURCE_DIR}/sample.toml
              $<TARGET_FILE_DIR:toml_sample>)
  endif()

endif()
