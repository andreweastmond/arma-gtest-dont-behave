cmake_minimum_required(VERSION 3.11 FATAL_ERROR)
project(armadillo_gtest LANGUAGES CXX)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Importing Armadillo when GTest is imported in the manner suggested by "CMake Cookbook" does not build.
find_package(Armadillo REQUIRED)
include_directories(${ARMADILLO_INCLUDE_DIRS})

add_library(armadillo_library using_armadillo.cpp)

add_executable(armadillo_gtest main.cpp)
target_link_libraries(armadillo_gtest armadillo_library)

# we will use the network to fetch Google Test sources
# make it possible to disable unit tests when not on network
option(ENABLE_UNIT_TESTS "Enable unit tests" ON)
message(STATUS "Enable testing: ${ENABLE_UNIT_TESTS}")

if(ENABLE_UNIT_TESTS)
    include(FetchContent)
    FetchContent_Declare(
            googletest
            GIT_REPOSITORY https://github.com/google/googletest.git
            GIT_TAG        release-1.8.0
    )
    FetchContent_GetProperties(googletest)
    if(NOT googletest_POPULATED)
        FetchContent_Populate(googletest)

        # Prevent GoogleTest from using PThreads
        set(gtest_disable_pthreads ON CACHE BOOL "" FORCE)

        # adds the targers: gtest, gtest_main, gmock, gmock_main
        add_subdirectory(
                ${googletest_SOURCE_DIR}
                ${googletest_BINARY_DIR}
        )

    endif()
    add_executable(cpp_test "")
    target_sources(cpp_test
                   PRIVATE
                   test.cpp)

    target_link_libraries(cpp_test
            PRIVATE
            armadillo_library
            gtest_main)

    enable_testing()

    add_test(NAME google_test
            COMMAND $<TARGET_FILE:cpp_test>)
endif()
