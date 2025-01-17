cmake_minimum_required(VERSION 3.5)

project(backtrace-libbun
    VERSION 0.1
    LANGUAGES C CXX
)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")

add_subdirectory(external/bcd)

find_package(libunwind)

if(NOT LIBUNWIND_FOUND AND LIBUNWIND_ENABLED)
    message(FATAL_ERROR "libunwind not found")
endif()

if(NOT DEFINED LIBUNWIND_ENABLED AND LIBUNWIND_FOUND)
    set(LIBUNWIND_ENABLED TRUE)
endif()

find_package(libbacktrace)

if(NOT LIBBACKTRACE_FOUND AND LIBBACKTRACE_ENABLED)
    message(FATAL_ERROR "libbacktrace not found")
endif()

if(NOT DEFINED LIBBACKTRACE_ENABLED AND LIBBACKTRACE_FOUND)
    set(LIBBACKTRACE_ENABLED TRUE)
endif()

if(ANDROID AND ANDROID_NDK_MAJOR)
    add_subdirectory("external/libunwindstack-ndk/cmake/")
endif()

add_subdirectory(src)

if(BUILD_TESTING)
    enable_testing()
    find_package(Threads REQUIRED)
    if (ANDROID AND ANDROID_NDK_MAJOR)
        set(GOOGLETEST_ROOT ${ANDROID_NDK}/sources/third_party/googletest)
        add_library(gtest STATIC ${GOOGLETEST_ROOT}/src/gtest_main.cc ${GOOGLETEST_ROOT}/src/gtest-all.cc)
        target_include_directories(gtest PRIVATE ${GOOGLETEST_ROOT})
        target_include_directories(gtest PUBLIC ${GOOGLETEST_ROOT}/include)
    elseif(NOT ANDROID)
        find_package(GTest REQUIRED)
    endif()
    add_subdirectory(tests)
endif()

if(BUILD_TOOLS)
    add_subdirectory(tools)
endif()

if(BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()
